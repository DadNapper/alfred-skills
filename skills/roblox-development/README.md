# Hermes Skills for Scraplands

Copy the **skill folders** inside this directory into your Hermes skills folder.

## Install

```bash
# Example: copy all skills into a personal skills directory
cp -R hermes/scraplands-* ~/.hermes/skills/
```

Or copy individual skills as needed. Each subfolder (`scraplands-*`) is one skill with a `SKILL.md`.

## Skills

| Folder | Use when |
|--------|----------|
| `scraplands-hermes` | **Always load** — role, repo path, read order, non-negotiables |
| `scraplands-telegram-topics` | Telegram topic detected — pick the right workflow mode |
| `scraplands-cursor-handoff` | Writing a Cursor implementation prompt |
| `scraplands-bug-triage` | QA, bugs, player feedback, regressions |
| `scraplands-git-workflow` | Commits, sync, ship — Alfred VPS git ops |

## Model routing

Hermes uses an **orchestrator pattern**: main session on GPT-5.5 (OpenAI Codex); cheap batch work via OpenRouter subagents and auxiliary slots.

| Doc | Purpose |
|-----|---------|
| **[`scraplands-hermes/references/model_routing.md`](scraplands-hermes/references/model_routing.md)** | **Canonical** — agent playbook (delegate vs orchestrate, responsibility → model) |
| `~/.hermes/model_routing.yml` | Human YAML index (not read by Hermes) |
| `~/.hermes/config.yaml` | **Enforcement** — `model`, `delegation`, `auxiliary`, `fallback_providers` |

Load `scraplands-hermes` before model-routing decisions; its workflow table points at `model_routing.md`.

## Repo is source of truth

These skills are a **routing layer**. Full project memory lives in the cloned repo:

```text
~/projects/scraplands/ai/
```

Run `scrapupdate` (or `git pull`) before tasks so Hermes reads current docs — do not maintain a second copy of `ai/` in skills.

## Updating

When `ai/` or `AGENTS.md` change in the repo, update the matching skill here if routing or non-negotiables changed, then re-copy to Hermes skills.

When model routing changes, update **both** `config.yaml` (Hermes runtime) and `scraplands-hermes/references/model_routing.md` (agent + git docs).
