---
name: scraplands-git-workflow
description: Alfred VPS git workflow for Scraplands. Use for repository sync, approved commits and pushes, branch safety, and operational git failures on ~/projects/scraplands.
---

# Scraplands Git Workflow (Alfred)

This is an Alfred-specific operational skill. Product reasoning, bug triage, implementation planning, and release judgment live in `DadNapper/game-dev-skills` and the active game repository.

Full operational documentation: repo `shellScripts/README.md`.

## Repo path

```text
~/projects/scraplands
```

## Task sequence

1. **Sync** — run `scrapupdate` before beginning operational work.
2. **Inspect** — confirm branch, tracking state, and working-tree status.
3. **Work** — do not pull, rebase, or merge in the middle of an active edit.
4. **Ship** — only after explicit approval, follow the game repo's release workflow and use `./shellScripts/ship.sh "commit message"`.
5. **Verify** — check `git status -sb` and `git log -1 --oneline`; do not trust a success echo alone.

## Never

- Force-push unless explicitly instructed
- Perform destructive git operations without explicit approval
- Commit or push because a task merely appears complete
- Replace product/release judgment supplied by ChatGPT or the game repository

## Pre-commit operational checks

- Debug and test overrides are disabled.
- Required project documentation and localization updates are present.
- Human Studio validation has happened when required by the game repository.
- The requested commit scope matches the actual diff.

## Push rejection recovery

If a commit was created but push was rejected because remote `main` moved:

1. Run `git fetch origin`.
2. Inspect `git status -sb` and `git log --oneline --decorate --left-right HEAD...origin/main`.
3. If local is exactly one approved Alfred commit ahead and only behind remote commits, rebase onto `origin/main`.
4. Push using `./shellScripts/push.sh`.
5. Verify clean tracking state and the latest commit SHA.

Do not force-push or reset to recover. Escalate on conflicts.

## Reporting

Report the command run, relevant error output, repository state, and the safest next action in the Telegram Agent Ops topic.
