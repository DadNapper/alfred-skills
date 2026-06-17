---
name: scraplands-telegram-operations
description: "Operate Scraplands work from Telegram topics: load topic markdown, route requests, manage active task files, and preserve repo workflow conventions."
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [scraplands, telegram, workflow, tasks, roblox]
---

# Scraplands Telegram Operations

Use this skill when Oz asks about Scraplands work from Telegram, especially when the request involves topics/threads, operational context, task tracking, feedback, analytics, publishing, QA, or implementation coordination.

## Core Rule

Do not rely only on Telegram chat history as memory. Scraplands has explicit runtime context in the repository. Before responding, identify the active Telegram topic/thread when available, then load the matching markdown file from:

```text
~/projects/scraplands/ai/telegram/topics/
```

Use that file as operational context for the response.

## Required Startup Checks

1. Treat `~/projects/scraplands` as the repo root.
2. Respect `AGENTS.md` and the existing Scraplands memories:
   - read `ai/README.md` and `ai/workflows/cursor_implementation.md` before implementation work
   - use `ai/engineering/` and relevant `Readmes/` for code-affecting tasks
   - preserve no-Rojo / Script Sync workflow
3. For Telegram topic behavior, load the relevant topic markdown under `ai/telegram/topics/`.
4. For implementation work, create or update a markdown task under:

```text
ai/tasks/active/
```

## Topic Routing

Current Scraplands topic files include:

- `active_dev.md` — execution-focused implementation work, Cursor prompts, branches, QA checklists, task states
- `agent_ops.md` — infrastructure chatter, automation failures, git issues, branch problems, operational setup
- `analytics.md` — telemetry, retention analysis, economy insights, trend detection, experiment recommendations
- `feedback_triage.md` — raw player/community feedback, pain points, qualitative signals, clustering, triage-doc linkage
- `ideas.md` — lightweight brainstorming and alternatives; do not jump immediately into implementation
- `publishing.md` — release summaries, risky systems, QA validation, conservative publish guidance
- `qa_bugs.md` — bug plans, severity, likely causes, debugging/QA plans, recent-change linkage

If a topic is renamed or split, re-list `ai/telegram/topics/*.md`, read the new files, and update this skill or its references.

## Completion / Main-Branch Update Requests

When Oz says an implementation is done and asks to mark it complete / update main:

1. Load the relevant topic context (`analytics.md`, `active_dev.md`, etc.) and repo workflow docs as usual.
2. Search both `ai/tasks/active/` and `ai/prompts/cursor/` for the matching task/plan. Some older implementation handoffs live under `ai/prompts/cursor/` rather than `ai/tasks/active/`.
3. If a matching handoff/plan exists outside `ai/tasks/active/`, update that existing artifact in place (for example add `Status: complete`, completion date, and implementation commit) rather than inventing a new active task just to close it.
4. If an `ai/tasks/active/*.md` task is marked `published`, `done`, or otherwise no longer active, move the markdown file to `ai/tasks/published/` (or `ai/tasks/done/` if that is the repo convention for the status) rather than only changing the status line in place.
5. Preserve unrelated untracked files. Do not stage or ship other active task files unless they are the thing Oz asked to complete.
6. Before pushing `main`, run the repo update path first (`./update.sh` or `./shellScripts/update.sh` when present), especially if Oz says “pull latest first”; then reconcile `origin/main` if local main is behind. A merge commit is acceptable when main has diverged and the repo workflow allows normal git commands; do not rebase or rewrite history without explicit Oz approval.
7. Ship with the repo wrapper (`./ship.sh` or `./shellScripts/ship.sh "Short descriptive message"` when present/verified) instead of ad-hoc `git add/commit/push`.
8. Verify with `git status --short --branch` after push. Report the pushed HEAD and any intentionally untouched local files.

## Active Task File Shape

When creating implementation task files, follow Oz's lightweight format rather than a giant design doc:

```markdown
# Task Title

Status: ready_for_cursor
Task ID: short-kebab-id
Branch: feature/short-kebab-id
Created: YYYY-MM-DD
Topic: Active Dev
Owner: Oz

## Goal

One concise outcome.

## Context

Why this matters / player or workflow signal.

## Cursor Prompt

Actionable implementation prompt with scope.

## Test Plan

- Specific checks
- Mobile / Studio / regression checks when relevant

## Completion Criteria

- Implementation completed
- Tested in Studio
- Ready for manual publish, if applicable

## Notes

Risks, trust/monetization warnings, or constraints.
```

See `references/topic-task-conventions.md` for the session-specific conventions learned from Oz's topic split and sample task.

## Feedback vs Analytics Split

Keep these separate:

## Ideas Topic Guardrail (Critical for Brainstorming)

When the active topic is `ideas.md` and the conversation proposes balancing or feature changes to existing systems (drones, bots, progression, economy, persistence-adjacent automation):

1. Do a quick grounding pass in existing project docs/config before giving recommendations:
   - `Readmes/` docs for the system area
   - `ai/memory/automation_systems.md` and related `/ai` system references
   - `Resources/Rebalance/*.json` for current tuning/config source-of-truth
2. Explicitly acknowledge known existing mechanics that affect the recommendation (for example existing efficiency/research tracks) so advice layers onto current architecture instead of duplicating it.
3. Prefer additive proposals that separate progression axes (coverage vs capability vs throughput) to reduce balance risk and avoid overloading a single upgrade path.

This keeps ideation lightweight while preventing "already exists" misses and preserves trust in design discussions.

- `feedback_triage.md` is qualitative. Append raw player/community feedback verbatim in its `Raw User Feedback` section when asked. Do not paraphrase in that raw section.
- `analytics.md` is quantitative/data-oriented. Discuss telemetry, retention, economy, progression trends, and experiments. Avoid overfitting to small samples.

## P0 Save-Loss Triage

When Oz reports Scraplands progress loss on leave/rejoin (money, miners/devices, inventory, world progress), treat it as P0 and use `references/p0-save-loss-triage.md`. Key workflow: append raw feedback, inspect persistence code before hypothesizing, look specifically for fire-and-forget leave saves, forced-save ordering races, pending-bucket cleanup, device debounce gaps, and disabled session-lock/rapid-rejoin risks, then create a focused active task under `ai/tasks/active/`.

## Player Kick / Disconnect Triage

When Oz asks why players are getting kicked, whether the server kicked them, or how to make disconnect handling more graceful, use `references/kick-disconnect-triage.md`. Key workflow: search explicit `Player:Kick()` paths, compare custom vs generic kick messages, preserve data/world/base protective kicks, and prefer warning-only handling for high-ping/network-quality systems.

## Badge Placeholder + Award Hook Pattern

When adding Roblox badges before Oz has created live badge IDs, use `references/badge-placeholder-award-pattern.md`. Key workflow: define placeholder IDs as `0`, keep runtime badge award definitions in `BadgeManager.luau`, mirror metadata in `Resources/scraplands_badges.json`, rely on `AwardBadge` skipping nonpositive IDs, and hook badge checks only after authoritative server state changes succeed.

## QA Bug Pattern: "Looks maxed but never reaches full stars"

When investigating research/progression bugs in `qa_bugs.md` where Oz reports a node appears capped at **4.5 stars** and never shows 5:

1. Inspect UI display math first (client): look for `displaySteps = math.min(earned, maxAllowed)` or equivalent clamping before rendering stars.
2. Verify gate math (shared logic): compare `earned` (owned steps) vs `getMaxAllowedResearchSteps(...)` (rebirth-gated cap).
3. Check whether the final step is gate-locked (common pattern: 10-step max where step 10 requires additional rebirths), which yields a persistent 4.5-star visual.
4. Confirm server purchase semantics (`maxed` vs `rebirth_required`) so copy/state does not imply true max when the last half-step is gate-blocked.
5. In reports to Oz, distinguish:
   - **True maxed** (owned max steps)
   - **Gate-capped** (owned/display capped below max until rebirth requirement is met)

Use this pattern before proposing art/UI fixes; this class of issue is usually progression gating surfacing through star rendering.

## Pitfalls

- Do not look for the old combined topic filename `feedback_and_analytics.md`; Oz split it into `feedback_triage.md` and `analytics.md`.
- Do not turn loose Ideas-topic conversation into an implementation task too early.
- Do not mark single loud feedback as a confirmed bug without evidence.
- Do not say publishing is safe without QA evidence.
- Do not create broad, vague active tasks; keep each task scoped and Cursor-ready.

## Verification Before Final Reply

Before replying to Oz:

- confirm which topic file was loaded, if topic context mattered
- confirm any renamed/split files by listing or reading the current repo state
- if implementation work was requested, confirm whether an `ai/tasks/active/*.md` task exists or was updated
- keep the final reply direct and operational
