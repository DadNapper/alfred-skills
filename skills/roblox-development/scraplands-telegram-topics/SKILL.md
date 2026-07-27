---
name: scraplands-telegram-topics
description: Route Alfred behavior for Scraplands Telegram threads, notifications, scheduled summaries, analytics, publishing updates, and agent operations.
---

# Scraplands Telegram Topics

This is an Alfred-specific communication and automation skill. General product reasoning, bug triage, Cursor handoffs, Roblox engineering guidance, and release assessment live in `DadNapper/game-dev-skills` and the active game repository.

Do not depend on hard-coded Telegram topic IDs. Route by the current thread title, the user's request, and recent thread context.

## Runtime routing

For each Telegram message:

1. Read the current thread/topic title when available.
2. Read the actual request and recent thread context.
3. Infer the operational mode from intent rather than topic ID.
4. Load the matching game-repository context under `ai/telegram/` when present.
5. If title and request disagree, prioritize the request.
6. Use shared workflows from `DadNapper/game-dev-skills` when the request needs development reasoning.
7. Write durable outcomes to the game repository; Telegram history is not project memory.

## Alfred-owned modes

- Notifications and alerts
- Scheduled summaries and reports
- Analytics collection and delivery
- Publishing-status communication
- Git/VPS/cron failures and recovery reporting
- Background data collection and cleanup

## Delegated reasoning

When a Telegram request needs one of these, use or reference the shared skill repository rather than maintaining copied instructions here:

- Project orchestration
- Product or roadmap advice
- Bug and feedback triage
- Cursor implementation handoffs
- Roblox architecture guidance
- Release-readiness decisions

## Operational reporting

Keep reports concise and include:

- What ran
- What changed or failed
- Evidence or relevant metrics
- Whether human action is required
- The safest next step

Keep infrastructure chatter in the Agent Ops topic when available.
