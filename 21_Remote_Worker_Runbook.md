---
status: canonical
last_verified: 2026-09-20
last_reviewed: 2026-09-28
---

# Remote Worker Runbook

This runbook covers the realistic operating model while Vuk is away from the Home PC.

It is intentionally small. The local helper is not expected to diagnose Linux, Docker, networking or the scraper project.

## Operating boundary

Last verified architecture:

```text
ThinkPad
  ↓
private Tailscale transport
  ↓
existing OpenSSH
  ↓
ypi-worker
  ↓
Docker Compose + persistent runtime volume
```

The 2026-09-20 acceptance test proved that this path recovered after a controlled reboot without local graphical login.

A later day's live state is still `UNKNOWN` until checked.

## If the Home PC appears to be off

The local helper instruction is:

> Press the power button once. Nothing needs to be typed.

Then allow the machine time to boot before testing remote access.

Do not ask the helper to open a terminal, run Docker commands or change settings.

## If Vuk explicitly determines the PC is frozen

Only when Vuk specifically instructs it:

> Hold the power button until the machine turns off, wait about 10 seconds, then press the power button once.

A forced power cycle is a recovery action, not a routine first step.

## ThinkPad connection check

From the ThinkPad:

```text
tailscale status
```

Then use the configured SSH alias:

```text
ssh ypi-worker
```

If that succeeds, ordinary host/service diagnosis can be done remotely.

## Minimal host health check

After SSH:

```bash
systemctl is-active tailscaled
systemctl is-active ssh
systemctl is-active NetworkManager
systemctl is-active docker
systemctl is-active containerd

df -h /
```

Expected normal service state is `active`.

When a task depends on persistent scraper state, also check the named runtime volume rather than creating a replacement volume.

## Project check

```bash
cd ~/Projects/scraper_project
git status --short --branch
docker compose config
```

Do not assume a local branch exists on GitHub. The bounded-Docker-logging branch was local-only at the last accepted checkpoint.

## Worker health

Use the existing project Compose/CLI boundaries rather than inventing a second worker process.

A safe health invocation is:

```bash
docker compose run --rm scraper worker health --runtime-dir /app/runtime
```

For a live source task, use only the command approved for that source/pilot. Do not substitute the synthetic worker command for a real-source pilot.

## Runtime protection rules

Never use these casually:

```text
docker compose down -v
docker volume prune
docker system prune --volumes
```

The named runtime volume holds the scraper SQLite state, checkpoints, snapshots and exports.

Do not move the validated runtime to the archive HDD as an ad-hoc remote fix.

## If SSH/Tailscale does not return after power-on

Possible failure domains include:

- Home PC did not boot successfully;
- home network/router is unavailable;
- ISP is unavailable;
- Tailscale service/account path is unavailable;
- OpenSSH failed;
- hardware failure.

If no in-band path exists, stop remote configuration attempts.

The local helper should perform only the simple power action Vuk requests. More complex boot/network repair waits for physical access unless a separate trusted person is intentionally guided through it.

## If the home router/ISP is down

The Home PC cannot repair an upstream home-network outage through Tailscale.

Do not reinterpret this as a scraper or Docker failure.

## Backup/recovery rule

A restore-tested off-machine checkpoint was verified on 2026-09-20.

Once real source data begins accumulating:

- create fresh coherent backups periodically;
- keep an off-machine copy;
- verify checksums;
- periodically prove restore into an isolated target.

The September checkpoint is proof that the recovery procedure works; it is not a forever-current backup.

## Before long unattended live jobs

Confirm:

- current remote access works;
- enough disk space exists;
- source-specific stop conditions exist;
- queue recovery/resume is tested for the actual live workflow;
- proxy-budget telemetry works;
- Docker log rotation is active on the deployed Compose configuration;
- a recent off-machine backup exists;
- no production secrets appear in Git or logs.

## Things not to change casually while remote-only

Avoid unnecessary:

- BIOS changes;
- kernel upgrades;
- NVIDIA driver changes;
- Docker storage relocation;
- firewall redesign;
- new orchestration platforms;
- destructive cleanup commands.

Preserve the known-good foundation unless a concrete problem requires a controlled change.
