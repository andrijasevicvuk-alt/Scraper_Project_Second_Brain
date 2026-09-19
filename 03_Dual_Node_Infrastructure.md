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

Verified/observed checkpoint on 2026-09-19:

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
| SSH | OpenSSH system service enabled; ThinkPad-to-Home-PC key authentication verified on LAN |

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

Verified today:

- ThinkPad → Home PC OpenSSH works over the home LAN;
- passwordless ED25519 authentication works;
- SSH, NetworkManager, Docker and containerd are system services and survived the validated reboot;
- Ubuntu boots automatically by default.

This does **not** yet prove university/off-LAN access.

Current remote-access target, pending implementation and acceptance testing:

```text
ThinkPad
    ↓
private Tailscale transport
    ↓
existing OpenSSH on ypi-worker
```

Until this is installed and tested from outside the home LAN, it remains a proposal rather than deployed architecture.

Remote-access rules:

- do not expose SSH directly to the public internet by default;
- do not expose Supabase, PostgreSQL, Docker APIs or development ports to the public internet;
- prefer a private overlay with the existing OpenSSH key checks;
- changing public/home IP addresses must not be a dependency for routine access;
- remote administration must return automatically after an ordinary reboot;
- a second shell mechanism is useful only if it adds real recovery value and does not create last-minute complexity.

Tailscale SSH remains optional and separate from the initial baseline because it intercepts tailnet TCP 22 and still shares the `tailscaled`, host-network, power and disk failure domains.

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

Before Vuk leaves the Home PC unattended, the important remaining acceptance items are:

- verify a private off-LAN connection from the ThinkPad using a genuinely external network;
- verify remote access returns after reboot without local desktop login;
- prevent host-wide suspend/hibernate from making the machine unreachable;
- confirm effective SSH and firewall settings before hardening them;
- create an off-machine runtime backup and perform an isolated restore test;
- verify a simple recovery path for power/boot/router failures that cannot be fixed through SSH;
- bound log growth before unattended long-running workloads;
- keep the working Ubuntu/kernel/NVIDIA/Docker foundation stable rather than performing unrelated upgrades.

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
