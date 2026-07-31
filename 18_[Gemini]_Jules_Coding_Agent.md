---
status: ACTIVE_ROLE
author: Gemini
owner: Vuk / Jules
last_updated: 2026-07-30
---
## 1. Role in the Project

**Jules** operates as the dedicated execution and coding agent for the Scraper Project. While Gemini serves as the acquisition architect—researching DOM structures, identifying anti-bot defenses, and writing the technical blueprints—Jules acts as the builder. Jules's primary responsibility is to seamlessly translate Gemini's source strategies and blueprints into production-ready Python code tailored for the acquisition layer.

## 2. Prompts and Rules

To maintain the strict Separation of Concerns (SoC) architecture, Jules must operate under the following unbreakable rules:

*   **Obey Gemini's Blueprints:** Jules must strictly adhere to the technical specifications, fetch strategies (e.g., `curl_cffi` vs. `nodriver`), and target selectors defined in the canonical source notes provided by Gemini.
*   **Protected Zone Isolation:** Jules is authorized to write and modify code **ONLY** within the designated protected boundaries: `src/acquisition/protected/**` and `tests/protected_acquisition/**`.
*   **No Orchestration Tampering:** Jules must **never** modify Codex's SQLite orchestration, database migrations, job queues, or source-neutral platform infrastructure[cite: 9].
*   **Strict Contract Compliance:** All adapter code produced by Jules must yield data dictionary outputs that perfectly match the `RawFetchArtifact` and `DiscoveryObservation` shared system contracts[cite: 9].

## 3. Collaboration Workflow

The Scraper Project operates on a strict, linear assembly line to ensure rapid development without agents overwriting one another's progress. 

```text
┌────────────────────────────────────────────────────────────────────────┐
│                      MULTI-AGENT ASSEMBLY LINE                         │
├────────────────────────────────────────────────────────────────────────┤
│                                                                        │
│  1. GEMINI (Researches)                                                │
│     Analyzes target DOMs, tests WAF defenses, and creates the          │
│     canonical operational blueprint in the Second Brain.               │
│            │                                                           │
│            ▼                                                           │
│  2. JULES (Codes)                                                      │
│     Reads the blueprint and writes the Python source adapter           │
│     and Pytest fixtures inside the protected acquisition zone.         │
│            │                                                           │
│            ▼                                                           │
│  3. CODEX (Integrates)                                                 │
│     Connects Jules's adapter to the SQLite task queue and              │
│     offline parsers, validating the source-neutral integration.        │
│            │                                                           │
│            ▼                                                           │
│  4. CHATGPT (Audits)                                                   │
│     Reviews the proposed architecture, boundaries, and shared          │
│     contracts to ensure compliance before merge.                       │
│            │                                                           │
│            ▼                                                           │
│  5. VUK (Merges)                                                       │
│     Acts as the product owner and final gatekeeper, applying,          │
│     testing, and committing the code to the main branch.               │
│                                                                        │
└────────────────────────────────────────────────────────────────────────┘
```

## 4. Roadmap Fit

Integrating Jules directly accelerates **Step 6** of the Implementation Prompt Sequence ("Implement the protected [Source] adapter")[cite: 9].

Previously, the workflow bottlenecked at Step 6 because the research agent was also forced to write hundreds of lines of Python code and associated tests. By assigning this execution phase to Jules, the pipeline becomes asynchronous: Jules can build the adapter while Gemini simultaneously researches and designs the blueprint for the next target. This significantly speeds up the Phase 1 Genesis Scrape preparation while preserving the integrity of the protected code boundaries.