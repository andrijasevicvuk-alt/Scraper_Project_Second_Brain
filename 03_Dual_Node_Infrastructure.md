# Dual-Node Infrastructure

The project uses two machines because the ThinkPad is the development and control machine, while the Home PC performs long and resource-heavy jobs.

For the period when Vuk is away from the Home PC, remote recoverability is part of the operating architecture. A technically correct worker is not considered operationally ready if a minor failure requires Vuk to be physically present.

## Control node — ThinkPad L15 Gen 2

Responsibilities:

- Obsidian second brain
- GitHub and branch management
- Codex sessions
- ChatGPT planning and review
- reviewing logs and summaries
- remotely starting or stopping jobs
- approving pilots and dataset batches
- checking proxy and storage budgets
- holding an off-machine copy of approved backups where appropriate

The ThinkPad should not be required to store every raw snapshot.

## Worker node — Home PC

Verified/observed checkpoint on 2026-09-20:

| Component | Specification / state |
|---|---|
| OS | Ubuntu 26.04.1 LTS, default boot OS; Windows 10 remains manually bootable |
| Kernel | `7.0.0-31-generic` |
| Hostname | `ypi-worker` |
| GPU | Zotac Gaming GeForce GTX 1660 / TU116 |
| CPU | AMD Ryzen 5 1600, 6 cores / 12 threads |
| Motherboard | ASUS Prime B450-Plus |
| RAM | 16 GB Corsair DDR4 |
| Power supply | Corsair CV650 |
| SSD | WDC 1 TB nominal / 931.5 GiB observed |
| HDD | Seagate 3 TB nominal / about 2.7 TiB observed; NTFS and currently unmounted |
| Docker | Native Docker Engine 29.8.1; Docker Compose 5.5.1; non-root use by `vuk` verified |
| SSH | OpenSSH enabled; ThinkPad key-only authentication verified on LAN and off-LAN through Tailscale; password and root SSH login disabled |

The earlier 2 TB HDD note was stale. The observed disk is approximately 3 TB nominal.

Responsibilities:

- Docker containers
- protected acquisition adapters
- Scrapling experiments when approved for a source
- browser processes
- proxy credentials
- persistent crawl queue
- checkpoints and runtime SQLite state
- raw snapshots
- parser batches
- scheduled routine runs when implemented
- local backup staging

## Bridge between the nodes

### GitHub

Use for:

- code
- tests
- migrations
- documentation
- configuration templates without secrets

Do not use for:

- `.env`
- proxy credentials
- runtime SQLite databases
- cookies or session material
- raw HTML archives
- runtime logs
- crawl checkpoints

### Secure remote access

Accepted operating baseline as of 2026-09-20:

```text
ThinkPad
    ↓
private Tailscale transport
    ↓
existing OpenSSH on ypi-worker
```

Verified evidence:

- Home PC Tailscale address: `100.108.39.117`;
- ThinkPad Tailscale address observed during acceptance: `100.117.143.124`;
- off-LAN access was tested from the ThinkPad while it used a phone hotspot rather than the home LAN;
- Tailscale reached `ypi-worker` through the Frankfurt DERP relay when a direct path was unavailable; this is an accepted fallback, not a failure;
- existing OpenSSH remained the shell service; Tailscale SSH stayed disabled;
- `tailscale up` was configured with Tailscale SSH disabled and DNS takeover disabled;
- ThinkPad ED25519 key authentication succeeded in BatchMode;
- password-only SSH was rejected with `Permission denied (publickey)`;
- root SSH login is disabled;
- the effective OpenSSH hardening drop-in is `/etc/ssh/sshd_config.d/00-remote-worker.conf`;
- a controlled Home-PC reboot returned networking, Tailscale and OpenSSH without any local graphical login;
- `tailscaled`, `ssh`, `NetworkManager`, `docker` and `containerd` were all verified active and enabled after reboot.

Effective OpenSSH policy accepted for this period:

```text
PubkeyAuthentication yes
PasswordAuthentication no
KbdInteractiveAuthentication no
PermitRootLogin no
```

Remote-access rules remain:

- do not expose SSH directly to the public internet by default;
- do not expose Supabase, PostgreSQL, Docker APIs or development ports to the public internet;
- use the private Tailscale transport with the existing OpenSSH key boundary;
- remote administration must return automatically after an ordinary reboot;
- Tailscale SSH remains optional and is not part of the current baseline.

### Host sleep policy

The Home PC must remain reachable while unattended. The accepted host-wide drop-in is:

`/etc/systemd/sleep.conf.d/90-remote-worker.conf`

with:

```text
[Sleep]
AllowSuspend=no
AllowHibernation=no
AllowHybridSleep=no
AllowSuspendThenHibernate=no
```

The effective configuration was rechecked after reboot. This is intentionally a host-level safeguard rather than a desktop-session preference.

### Runtime storage

The current Step 3 runtime is in the Docker named volume:

`scraper_project_scraper-runtime`

Inside the container the verified layout is:

```text
/app/runtime/
├── database/
│   └── scraper.sqlite
├── checkpoints/
├── snapshots/
├── logs/
└── exports/
```

Do not replace this with the older `cache.db` example path when documenting current runtime state.

Do not move the validated runtime to the HDD as part of the pre-departure reliability work.

## Verified Step 3 dual-node evidence

Step 3B physically demonstrated:

- ThinkPad control of the Home PC through SSH;
- native non-root Docker operation;
- synthetic worker execution;
- persistent SQLite/runtime state across container recreation;
- persistence across a full physical Home-PC reboot;
- same-run idempotency without duplicate snapshot/batch creation;
- abandoned leased-job recovery after lease expiry;
- Ubuntu automatic default boot;
- successful manual Windows 10 boot and subsequent automatic return to Ubuntu.

These results validate the Step 3 worker architecture. They do not by themselves prove month-long unattended remote readiness.

## Pre-departure remote-readiness acceptance

**Status: accepted on 2026-09-20 with a documented physical fallback boundary.**

The reliability gate created after Step 3 is no longer blocking Step 4. Acceptance evidence includes:

- genuinely off-LAN private Tailscale connectivity from the ThinkPad;
- OpenSSH key-only access over that private path;
- password and root SSH login disabled;
- remote access returning after a controlled Home-PC reboot without local GUI login;
- host-wide suspend/hibernate prevention;
- `tailscaled`, `ssh`, `NetworkManager`, `docker` and `containerd` active and enabled after reboot;
- approximately 352 GiB free on the Ubuntu root filesystem at final acceptance;
- a coherent runtime backup created with the project's SQLite backup mechanism;
- SHA-256 verification of the backup artifacts;
- an off-machine copy stored on the ThinkPad;
- an isolated restore through the project's restore helper;
- `PRAGMA integrity_check` returning `ok` on both the backup and restored database;
- migration state `0001` and `0002` preserved in the restored database;
- a simple physical-helper procedure for failures that cannot be solved in-band.

Verified backup locations at acceptance:

```text
Home PC:
/home/vuk/Backups/ypi/20260920-035331

ThinkPad:
C:\Users\HT-ICT\YPI-Backups\20260920-035331

Isolated restore test:
/home/vuk/RestoreTests/20260920-035331
```

The runtime backup was approximately 208 KiB and included the SQLite database, snapshots, exports, logs, empty checkpoints directory, inventory and checksum manifest.

### Docker log-growth control

A scoped Compose change adding per-service `json-file` limits of `max-size: "10m"` and `max-file: "3"` was validated and committed locally in `scraper_project` on branch `chore/bounded-docker-logging`, commit `85329f6`.

At acceptance time that branch had not yet been merged into `main`, and no scraper Compose services were running. Therefore:

- no containers were recreated;
- the named runtime volume identity remained unchanged;
- the runtime SQLite `PRAGMA quick_check` returned `ok`;
- the logging limits should not be treated as active on `main` until the branch is reviewed and merged.

## Failure domains and recovery boundary

Remote administration depends on more than SSH. A working remote path still depends on:

- Home PC power;
- successful Ubuntu boot;
- host networking;
- home router/ISP availability;
- the private-overlay service once deployed;
- OpenSSH;
- valid identity/key state;
- sufficient disk space.

If the OS or network is alive, Vuk should be able to recover ordinary service failures remotely. If the PC is powered off, frozen, unable to boot, or the home network is down, an in-band shell cannot recover it. A trusted local person or independently tested out-of-band mechanism remains the physical recovery boundary.

Physical helper instructions should stay minimal. Do not require the helper to use Linux commands or diagnose the project.

## Operational requirements before live Genesis

In addition to the remote-readiness gate, before a live Genesis run prove:

- a job can be started remotely;
- a job can be stopped gracefully;
- a worker restart does not lose queue state;
- partial progress is preserved;
- snapshots remain accessible after a container rebuild;
- credentials never enter Git;
- disk-space and proxy-budget warnings work;
- backups and restore procedures match the production runtime.

## Concurrency starting point

Start conservatively:

- one active source;
- one browser session by default;
- one authoritative queue owner;
- bounded retries.

Increase concurrency only after measuring memory, CPU, error rate and proxy use.
