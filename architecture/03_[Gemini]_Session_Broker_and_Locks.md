---
status: ACTIVE_DESIGN
author: Gemini
domain: Session Reliability
last_updated: 2026-07-31
---
## 1. The Session Broker Pattern

When scraping 100,000+ active listings on aggressively protected targets (e.g., Boat24, iNautia), the extraction fleet relies on `cf_clearance` cookies and forged TLS fingerprints. Because these cookies naturally expire or get invalidated by Cloudflare, our fast `curl_cffi` workers will inevitably encounter HTTP 403 blocks mid-crawl.

To prevent 50 concurrent workers from all crashing or trying to spawn 50 headless browsers simultaneously (the "thundering herd" problem), we implement a **Session Broker Pattern**. 

The broker centralizes the management of the **Identity Bundle** (Cookies, User-Agent, and JA4 TLS parameters). It tracks these bundles using a **Session Generation Number**—a monotonically increasing integer. Every worker caches the current generation number in memory; if the central generation number increments, the worker knows its local bundle is stale and reloads the fresh credentials from the database before making its next request.

## 2. SQLite Distributed Lock

To respect our established infrastructure boundaries and minimize external dependencies, this system will **not** use Redis for distributed locking. 

As formalized in **Decision D-008**[cite: 15], we rely strictly on an SQLite runtime persistence foundation utilizing WAL mode and a one-writer policy[cite: 15]. The Session Broker will use a dedicated `session_leases` or `session_locks` table within this SQLite database to act as our concurrency mutex. 

This table will track:
*   `lock_status`: Boolean indicating if a refresh is currently in progress.
*   `locked_by_worker_id`: Identifier of the worker executing the refresh.
*   `expires_at`: A strict timeout (e.g., 60 seconds) to ensure the system self-heals if the refreshing worker crashes mid-execution.

## 3. The Refresh Flow

When a security blockade is triggered, the engine executes a precise, coordinated recovery sequence to safely re-authenticate and resume the scrape.

```text
┌─────────────────────────────────────────────────────────────────────────┐
│                   AUTOMATED SESSION REFRESH FLOW                        │
├─────────────────────────────────────────────────────────────────────────┤
│                                                                         │
│  [Worker A]            [Worker B]             [Worker C]                │
│      │                     │                      │                     │
│      ▼                     ▼                      ▼                     │
│ (Hits HTTP 403)       (Hits HTTP 403)        (Hits HTTP 403)            │
│      │                     │                      │                     │
│      └───► ALL: Check SQLite `session_locks` Table ◄┘                   │
│                            │                                            │
│                [Lock Acquired by Worker A]                              │
│                            │                                            │
│             ┌──────────────┴──────────────┐                             │
│             │ `nodriver` Stealth Session  │                             │
│             │ 1. Solve JS/Turnstile       │  (Workers B & C pause and   │
│             │ 2. Execute Health Check     │   poll SQLite with random   │
│             │ 3. Save New Cookie Bundle   │   jittered sleep timers)    │
│             │ 4. Increment Generation +1  │                             │
│             │ 5. Release SQLite Lock      │                             │
│             └──────────────┬──────────────┘                             │
│                            │                                            │
│        ┌───────────────────┴───────────────────┐                        │
│        ▼                                       ▼                        │
│   [Worker A]                        [Workers B & C]                     │
│ Resume Fetching               Detect Generation Change in DB            │
│                               Reload New Identity Bundle & Resume       │
│                                                                         │
└─────────────────────────────────────────────────────────────────────────┘
```

### Step-by-Step Sequence:

1. **Block Detected:** Worker receives an HTTP 403, 429, or Turnstile challenge page. It immediately aborts its current fetch task and returns the job to the queue.
    
2. **Lock Check:** The worker checks the SQLite `session_locks` table.
    
3. **Lock Acquisition:** If the lock is open, the worker acquires it. If it is already locked by another worker, the current worker enters a sleep loop with **randomized jitter** (e.g., `sleep(uniform(2.0, 5.0))`) to prevent simultaneous database hammering.
    
4. **Stealth Re-Authentication:** The worker holding the lock spins up the `nodriver` Stealth Cookie Generator, navigates the challenge, and extracts the new clearance cookies.
    
5. **Health Check:** Before saving, the `nodriver` instance hits a verification endpoint to prove the new cookie bypasses the WAF.
    
6. **State Update:** The valid identity bundle is committed to the SQLite store, the **Session Generation Number** is incremented (`Gen N` $\rightarrow$ `Gen N+1`), and the lock is released.
    
7. **Fleet Resumption:** The sleeping workers wake up, notice the Generation Number has changed, load the new cookies into their `curl_cffi` instances, and seamlessly resume draining the target URL queue.