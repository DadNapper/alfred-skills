# Alfred Automation Skills

Alfred is now the automation and notification layer, not the primary game-development reasoning layer.

Reusable game-development workflows have moved to:

```text
DadNapper/game-dev-skills
```

## Alfred should keep

- Telegram topic routing, summaries, and notifications
- Scheduled jobs and cron-driven monitoring
- VPS-specific operations
- Repository sync and operational git helpers used by Alfred
- Roblox analytics collection and automated reporting
- Background data collection, cleanup, and alerting

## Alfred should not duplicate

- Project orchestration
- Product and roadmap advice
- General bug or feedback triage
- Cursor implementation handoffs
- General Roblox engineering guidance
- Release-readiness reasoning
- Per-game product memory
- `SOUL.md` content as a shared development workflow

## Source-of-truth model

- Shared workflows: `DadNapper/game-dev-skills`
- Game-specific truth: each game's `AGENTS.md` and `ai/` directory
- Alfred-specific automation: this repository

Alfred may invoke or link to the shared workflows, but it should not maintain copied versions of them. This keeps Telegram and background automation useful without creating a second reasoning system that drifts from ChatGPT and the game repositories.
