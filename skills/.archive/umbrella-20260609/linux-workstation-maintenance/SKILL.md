---
name: linux-workstation-maintenance
description: "Audit and set up conservative cron-based maintenance for Linux AI workstations/servers: updates, healthchecks, log cleanup, backups, Docker cleanup, and rollback-friendly logging."
version: 1.0.0
author: Hermes Agent
license: MIT
platforms: [linux]
metadata:
  hermes:
    tags: [linux, ubuntu, cron, maintenance, backups, healthcheck, logging, pm2, docker]
---

# Linux Workstation Maintenance

Use this skill when asked to maintain an Ubuntu/Linux workstation or server with scheduled jobs, cron audits, update scripts, healthchecks, backup scripts, log cleanup, Docker cleanup, or recovery automation.

This is especially relevant for AI workstations/servers that run Hermes, gateway processes, PM2-managed services, Docker workloads, or long-running local automation.

## Principles

- Be conservative and production-safe.
- Audit existing schedules before creating anything.
- Avoid duplicate cron entries.
- Prefer structured scripts with comments over inline crontab one-liners.
- Use absolute paths in cron scripts.
- Append timestamped logs to a dedicated log directory.
- Make scripts fail gracefully and never hang unattended.
- Detect optional tools before using them: `pm2`, `docker`, `jq`, `curl`, etc.
- Never run Ubuntu major-version upgrades from maintenance scripts.
- Never delete active data; rotate, archive, or skip instead.
- Make rollback easy by backing up configs and crontabs before/alongside changes.

## Standard workflow

1. Inspect existing schedules first.
   - User crontab: `crontab -l 2>/dev/null || true`
   - System crontab: `/etc/crontab`
   - System snippets: `/etc/cron.d/*`
   - Periodic dirs: `/etc/cron.hourly`, `/etc/cron.daily`, `/etc/cron.weekly`, `/etc/cron.monthly`
   - Systemd timers: `systemctl list-timers --all --no-pager`
   - If Hermes internal cron may exist, use the Hermes cron tool or `hermes cron list` when appropriate.

2. Identify and preserve existing user-requested jobs.
   - Do not duplicate existing update/healthcheck/cleanup jobs.
   - Do not modify existing Hermes update cadence unless explicitly asked.
   - If adding a crontab block, use a managed begin/end marker so future removal is clean.

3. Create a structured maintenance tree.
   - `~/maintenance/scripts/`
   - `~/maintenance/logs/`
   - `~/maintenance/backups/`
   - Set backups private, e.g. `chmod 700 ~/maintenance ~/maintenance/backups`.

4. Write scripts defensively.
   - `#!/usr/bin/env bash`
   - `set -uo pipefail` rather than brittle `set -e` everywhere.
   - Create log dirs at runtime.
   - Use `flock` locks to avoid overlapping runs.
   - Use a `log()` helper that prefixes `date -Iseconds`.
   - Use absolute paths for cron reliability.
   - For sudo apt jobs, use `sudo -n`; if passwordless sudo is unavailable, log and exit instead of hanging.

5. Install cron entries idempotently.
   - Remove only the previous managed block and duplicate exact script entries.
   - Reinstall a fresh managed block.
   - Do not touch unrelated crontab lines.

6. Verify after installation.
   - `bash -n` all scripts.
   - Run non-destructive scripts once if safe: healthcheck, log cleanup, backup.
   - Confirm script permissions.
   - Confirm each cron entry appears exactly once.
   - Print final crontab and warnings.

## Common schedules

Good defaults for a low-maintenance workstation/server:

- Healthcheck: every 15 minutes: `*/15 * * * *`
- Ubuntu updates: Sunday 04:00: `0 4 * * 0`
- Log cleanup: Sunday 04:30: `30 4 * * 0`
- Config backup: Sunday 05:00: `0 5 * * 0`
- Docker cleanup: first day of month 03:00: `0 3 1 * *`

Stagger jobs so update, cleanup, backup, and Docker maintenance do not race.

## Script patterns

### Logging helper

```bash
LOG_DIR="/home/alice/maintenance/logs"
LOG_FILE="$LOG_DIR/job_name.log"
DATE="/usr/bin/date"
MKDIR="/usr/bin/mkdir"
$MKDIR -p "$LOG_DIR"

log() {
  printf '[%s] %s\n' "$($DATE -Iseconds)" "$*" >> "$LOG_FILE"
}
```

### Lock pattern

```bash
LOCK_FILE="/tmp/job_name.lock"
FLOCK="/usr/bin/flock"
exec 9>"$LOCK_FILE"
if ! $FLOCK -n 9; then
  log "another run is already active; exiting."
  exit 0
fi
```

### Sudo apt safety

```bash
if ! /usr/bin/sudo -n true >> "$LOG_FILE" 2>&1; then
  log "ERROR: passwordless sudo unavailable; apt maintenance skipped safely."
  exit 1
fi

export DEBIAN_FRONTEND=noninteractive
/usr/bin/sudo -n /usr/bin/apt update >> "$LOG_FILE" 2>&1
/usr/bin/sudo -n /usr/bin/apt upgrade -y >> "$LOG_FILE" 2>&1
/usr/bin/sudo -n /usr/bin/apt autoremove -y >> "$LOG_FILE" 2>&1
```

## Healthcheck guidance

For Hermes-like services:

- Check process existence with `pgrep -af` against stable command substrings.
- Check PM2 only if `command -v pm2` succeeds; cron may have a smaller PATH, so also check known user-local PM2 paths such as `~/.local/bin/pm2` and a configured npm prefix.
- When the user asks for a PM2 process named `hermes`, make the healthcheck explicitly target `pm2 restart hermes` rather than auto-detecting a different process name.
- Discover PM2 status with `pm2 jlist` plus `jq` when available; require the target process to exist and be `online`.
- Restart via PM2 only when the PM2-managed process is actually found.
- Check ports only when a configured port is known; otherwise log that port checking was skipped.
- Support explicit overrides like `HERMES_HEALTH_PORT`, but avoid treating one missing port variable as a hard failure.

Do not invent fallback restart commands that may spawn duplicate daemons. If PM2/systemd is not configured, log a recommendation instead.

## PM2 handoff guidance for Hermes gateway

When migrating an already-running Hermes gateway to PM2, identify the existing supervisor and startup command first:

```bash
systemctl cat hermes-gateway.service
systemctl show hermes-gateway.service -p ExecStart -p WorkingDirectory -p Environment --no-pager
ps -eo pid,ppid,user,lstart,comm,args | grep -E '[h]ermes_cli\.main gateway run|[h]ermes.*gateway run'
```

A systemd-managed Hermes gateway commonly runs:

```bash
/home/USER/.hermes/hermes-agent/venv/bin/python -m hermes_cli.main gateway run --replace
```

Use a PM2 ecosystem file rather than a long one-off command. The app should usually be named `hermes`, set `cwd` to the Hermes source directory, run the venv Python with `-m hermes_cli.main gateway run --replace`, set `interpreter: "none"`, and include `HOME`, `USER`, `LOGNAME`, `HERMES_HOME`, `VIRTUAL_ENV`, and a full PATH.

Safe cutover sequence:

1. Install PM2 with the existing Node/npm setup, e.g. `npm install -g pm2`.
2. Verify `pm2 --version`; if npm's global bin directory is not on PATH, add it or symlink PM2 into `~/.local/bin`.
3. Create the PM2 ecosystem config.
4. Stop/disable the old supervisor before starting PM2. Do not run systemd and PM2 supervising the same `hermes gateway run --replace` command at the same time; `--replace` can make them fight each other.
5. Start: `pm2 start /path/to/ecosystem.config.js`.
6. Verify: `pm2 list`, `pm2 describe hermes`, and `pm2 logs hermes --lines 50`.
7. Save: `pm2 save`.
8. Configure reboot persistence with `pm2 startup systemd -u USER --hp /home/USER`, then run the exact sudo command PM2 prints.
9. Verify restart behavior with `pm2 restart hermes` and confirm logs still append.

If the old system service cannot be stopped/disabled because sudo or approval is unavailable, do not claim PM2 is managing production. Report the exact blocked handoff and leave the existing supervisor in place.

## Backup guidance

Backups should be timestamped and never overwritten.

Include, when present and safe:

- Hermes config files: `~/.hermes/config.yaml`, `~/.hermes/.env`, `~/.hermes/auth.json`
- Hermes package manifests: `package.json`, `package-lock.json`
- Maintenance scripts
- User crontab
- Readable system cron text

Use `umask 077` because backups can include secrets.

Avoid bulky session/log directories unless explicitly requested.

## Log cleanup guidance

- Rotate/compress oversized logs rather than deleting active logs.
- Delete only old rotated/compressed logs older than the retention window, commonly 30 days.
- Flush PM2 logs only if PM2 is installed.
- Avoid deleting app data, current logs, Docker volumes, or databases.

## Docker cleanup guidance

- Run Docker cleanup only if `docker` exists and `docker info` succeeds.
- `docker system prune -f` is reasonable conservative cleanup.
- `docker image prune -f` removes dangling images.
- Do not prune volumes unless the user explicitly asks.
- Log `docker system df` before and after.
- If a system cron already prunes Docker images, mention the overlap.

## Final response checklist

After setup, report:

- Existing schedules found and preserved.
- Final installed cron entries.
- Paths of all created scripts.
- Verification performed and real output summary.
- Warnings/recommendations such as missing PM2, missing passwordless sudo, unknown port, Docker permission issues, or overlapping existing Docker prune jobs.
- Exact removal instructions, preferably removing the managed block between begin/end markers.

## Reference notes

- `references/hermes-workstation-cron-maintenance.md` — concrete session-derived example of a Hermes AI workstation maintenance setup.
- `references/hermes-pm2-handoff.md` — PM2 handoff details for a Hermes gateway, including startup-command discovery, ecosystem config shape, PATH quirks, and the systemd double-supervision pitfall.