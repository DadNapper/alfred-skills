---
name: autonomous-coding-agents
description: Use when delegating coding work to autonomous CLI agents such as Claude Code, Codex, or OpenCode. Provides one orchestration workflow with tool-specific subsections and preserved reference packages.
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [agents, coding, codex, claude-code, opencode, delegation]
    related_skills: []
---

# Autonomous Coding Agents

## Overview

Use this umbrella when a coding task is large enough to delegate to an external autonomous CLI agent. The shared class workflow matters more than the individual tool: define scope, isolate the worktree, pass self-contained context, require verification output, and review changes before claiming success.

Original tool-specific packages are preserved under `references/original-skills/<skill-name>/`.

## When to Use

- The user asks to use Claude Code, Codex, OpenCode, or an autonomous agent.
- A coding task benefits from an isolated implementer plus parent-agent review.
- You need parallel exploration or implementation attempts.

## Shared Workflow

1. Confirm repository, branch, worktree state, and tests.
2. Create a self-contained brief: goal, constraints, files, commands, and expected verification.
3. Run the selected CLI in an isolated context; do not assume it succeeded from its prose.
4. Inspect `git diff`, run tests/builds yourself, and fix integration issues.
5. Summarize concrete changes and real verification output.

## Tool-Specific Subsections

### Claude Code
Best for broad codebase edits and long-context reasoning. Preserve strict prompts and require it to leave a reviewable diff.

### Codex
Best for focused code tasks and agentic terminal work. Keep prompts explicit about tests and non-interactive operation.

### OpenCode
Best when the user's environment is configured for OpenCode or when its model/tooling is preferred. Verify outputs the same way as any other agent.

## Verification Checklist

- [ ] Parent agent inspected the final diff.
- [ ] Tests/builds were run outside the child agent.
- [ ] Any external side effect has a verifiable handle.
- [ ] Tool-specific details are consulted from `references/original-skills/` when needed.
