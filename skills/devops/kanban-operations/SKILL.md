---
name: kanban-operations
description: Use when running Hermes Kanban orchestration, worker lanes, or board-driven task execution. Consolidates orchestrator and worker procedures into one class-level operations skill.
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [kanban, orchestration, workers, devops]
    related_skills: [autonomous-coding-agents]
---

# Kanban Operations

## Overview

Use this umbrella for Kanban-driven task execution: orchestrating boards, routing cards, running workers, and coordinating lane-specific agents. Orchestrator and worker behavior are two roles in the same class of workflow, so keep them discoverable together.

Original packages are preserved under `references/original-skills/<skill-name>/`.

## When to Use

- The user asks to run or debug the Kanban orchestrator.
- The user asks about a Kanban worker lane or card execution.
- A task involves board state, lane assignment, worker prompts, or handoff conventions.

## Role Subsections

### Orchestrator
Owns intake, decomposition, lane assignment, dependency tracking, and final integration. It should keep prompts self-contained and verify worker outputs.

### Worker
Owns executing one card at a time, respecting card acceptance criteria, and returning concrete evidence: files changed, tests run, blockers, and next handoff.

## Operating Principles

- Keep board state as the source of truth.
- Avoid recursive scheduling unless explicitly part of the Kanban system.
- Treat worker summaries as untrusted until verified.
- Capture reusable lane prompts under templates rather than new one-off skills.

## Verification Checklist

- [ ] Board/card state read before acting.
- [ ] Role-specific instructions matched the card.
- [ ] Worker output verified by artifact, diff, log, or status.
