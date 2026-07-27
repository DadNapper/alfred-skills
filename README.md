# Alfred Skills

Operational skills for Alfred, the always-on automation layer used alongside ChatGPT, GitHub, and game repositories.

This repository is the shared **automation-and-delivery layer**. Alfred handles scheduled jobs, Telegram routing, VPS operations, monitoring, repository synchronization, and notifications. Product reasoning and reusable game-development workflows live in `DadNapper/game-dev-skills` instead.

## Use this repository when

- Routing incoming Telegram messages to the correct operational mode
- Sending scheduled summaries, alerts, or progress reports
- Running read-only analytics or health-monitoring jobs on a VPS
- Synchronizing a game repository before automation runs
- Performing approved commit-and-push workflows from Alfred’s environment
- Reporting failures from cron jobs, API checks, or repository operations

### Sample uses

- Send a morning Telegram digest of new player feedback and current analytics.
- Alert when a Roblox metric or server health check crosses a configured threshold.
- Pull the latest clean repository state before a scheduled report runs.
- Use the approved ship script after human testing and explicit permission to commit and push.
- Route a Telegram thread about publishing into release-reporting mode rather than product-design mode.

## Do not use this repository for

- Product strategy, feature prioritization, economy design, or architecture decisions
- General bug triage or Cursor handoff workflows
- Reusable Luau packages
- New-game repository scaffolding
- Game-specific source of truth that belongs in a game repository
- Agent personality or duplicated project memory

## Current boundary

Alfred should remain lightweight and operational:

```text
ChatGPT + game-dev-skills
  reasoning, planning, triage, handoffs, GitHub and document work

Game repository
  code, architecture, priorities, decisions, and durable project truth

Alfred + alfred-skills
  Telegram, schedules, monitoring, VPS operations, notifications, and approved shipping
```

## Retained skills

The active Roblox-development automation skills currently include:

- `scraplands-telegram-topics` — infer operational behavior from Telegram thread intent
- `scraplands-git-workflow` — safe VPS sync, approved shipping, and Git failure reporting

Additional monitoring or scheduled-delivery skills belong here when they require Alfred’s always-on runtime.

## Operating rules

- Treat Telegram history as transient; write durable outcomes to the relevant game repository.
- Read the active game repository before reporting project state.
- Do not duplicate workflows already maintained in `game-dev-skills`.
- Do not make product or architecture decisions merely because an automation triggered.
- Never force-push, reset, discard local work, or publish without explicit approval.
- Scheduled jobs should fail visibly and report the command, error, and safest next action.

## Related repositories

| Repository | Responsibility |
|---|---|
| `DadNapper/game-dev-skills` | Shared reasoning, triage, handoff, and release workflows |
| `DadNapper/roblox-game-kit` | Reusable Roblox Luau packages |
| `DadNapper/roblox-script-sync-template` | New-game repository scaffold |
| `DadNapper/game-dev-workspace` | Safe multi-repository local operations |
| Individual game repository | Game-specific production truth |
| `DadNapper/alfred-skills` | Alfred-specific automation and delivery workflows |
