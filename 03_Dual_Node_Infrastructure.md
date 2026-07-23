# Dual-Node Infrastructure

The project uses two machines because the ThinkPad is the development and control machine, while the home PC performs long and resource-heavy jobs.

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

The ThinkPad should not be required to store every raw snapshot.

## Worker node — Home PC

Current known specifications:

| Component | Specification |
|---|---|
| GPU | Zotac Gaming GeForce GTX 1660, likely 6 GB |
| CPU | AMD Ryzen 5 1600, 6 cores / 12 threads |
| Motherboard | ASUS Prime B450-Plus |
| RAM | 16 GB Corsair DDR4 |
| Power supply | Corsair CV650 |
| SSD | 1 TB |
| HDD | 2 TB |

Responsibilities:

- Docker containers
- protected acquisition adapters
- Scrapling experiments
- browser processes
- proxy credentials
- persistent crawl queue
- checkpoints and `cache.db`
- raw snapshots
- parser batches
- scheduled routine runs
- local backup copies

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
- `cache.db`
- cookies or session material
- raw HTML archives
- runtime logs
- crawl checkpoints

### Secure remote access

Use for:

- starting jobs
- reading status
- stopping jobs
- retrieving summaries
- checking disk and process health

### Runtime storage

Recommended worker layout:

```text
runtime/
├── cache.db
├── checkpoints/
├── snapshots/
├── logs/
├── exports/
└── backups/
```

## Operational requirements

Before a live Genesis run, prove:

- a job can be started remotely
- a job can be stopped gracefully
- a worker restart does not lose queue state
- partial progress is preserved
- snapshots remain accessible after a container rebuild
- credentials never enter Git
- disk-space and proxy-budget warnings work

## Concurrency starting point

Start conservatively:

- one active source
- one browser session by default
- one authoritative queue owner
- bounded retries

Increase concurrency only after measuring memory, CPU, error rate and proxy use.
