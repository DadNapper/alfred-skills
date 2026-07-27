---
name: game-project-orchestration
description: Coordinate work across game repositories by loading project-local truth, selecting the right workflow, and preserving human approval gates.
---

# Game Project Orchestration

Use this skill for planning, triage, feature shaping, implementation handoffs, and project-state updates across any game.

## Operating model

1. Identify the active game and repository.
2. Read `AGENTS.md` and `ai/GAME_PROFILE.md`.
3. Read current priorities, relevant decisions, and only the documentation needed for the task.
4. Select the applicable core and platform workflows.
5. Recommend the simplest safe approach.
6. Keep implementation, testing, commit, and publish gates explicit.
7. Write durable decisions and task state back to the game repository.

## Responsibilities

- Plan and scope work
- Challenge weak technical or product assumptions
- Separate symptoms from root causes
- Protect architecture and persistence constraints
- Consider player experience, performance, security, analytics, rollout, and rollback
- Produce implementation-ready handoffs when another coding agent is doing the work

## Guardrails

- Do not assume one game's rules apply to another.
- Do not duplicate project memory into shared skills.
- Do not invent missing architecture or repo paths.
- Do not commit, merge, or publish unless explicitly requested.
- When implementation is already happening in another coding tool, default to diagnosis and a bounded handoff rather than competing edits.

## Decision checks

Before recommending a solution, ask:

1. Is it the simplest viable solution?
2. Is it secure and maintainable?
3. Does it fit the game's declared architecture?
4. Is it performant on the target platforms?
5. Can it be tested, observed, rolled back, and safely shipped?
6. Is the complexity justified by player or business impact?
