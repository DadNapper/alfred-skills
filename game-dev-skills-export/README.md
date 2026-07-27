# Game Development Skills

Reusable workflows for designing, building, operating, and shipping multiple games.

This directory is a migration-ready export intended to become the standalone `DadNapper/game-dev-skills` repository.

## Principle

**Skills explain how to work. Game repositories explain what is true.**

Shared skills must not contain game-specific architecture, feature history, repo paths, personalities, model-provider routing, or private project memory.

## Structure

- `core/project-orchestration` — identify the active project and route work
- `core/solo-dev-advisor` — prioritization and studio-building principles
- `core/bug-triage` — evidence-based defect and feedback triage
- `core/cursor-handoff` — implementation-ready coding handoffs
- `core/release-readiness` — validation, rollout, rollback, and observability
- `platforms/roblox` — reusable Roblox engineering constraints
- `templates/GAME_PROFILE.template.md` — per-game context contract

## Expected game repository contract

Each game should own:

```text
AGENTS.md
ai/
  GAME_PROFILE.md
  current_priorities.md
  decisions.md
  features/
  tasks/
```

Before substantial work:

1. Identify the active repository.
2. Read `AGENTS.md`.
3. Read `ai/GAME_PROFILE.md`.
4. Read only the relevant project documentation.
5. Apply the appropriate shared and platform workflows.
6. Store durable project decisions back in the game repository.

## Explicit exclusions

This package does not include:

- `SOUL.md` or agent personality
- Telegram routing
- VPS paths or cron jobs
- provider/model cost routing
- duplicated game memory
- automatic commit or publish authority
