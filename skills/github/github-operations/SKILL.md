---
name: github-operations
description: Use when managing GitHub repositories, auth, issues, PRs, reviews, CI, or repository inspection. Consolidates narrow GitHub workflow skills into one discoverable operations umbrella.
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [github, gh, pull-requests, issues, repositories, review]
    related_skills: []
---

# GitHub Operations

## Overview

Use this class-level skill for GitHub work: authentication, repository setup, issue management, pull-request lifecycle, code review, CI triage, and codebase inspection. These tasks share the same prerequisite checks: confirm repository/remotes, check `gh auth status`, inspect git state before edits, make side effects explicit, and verify with `gh`/git output.

Original source packages are preserved under `references/original-skills/<skill-name>/`.

## When to Use

- The user asks to clone, create, fork, or configure a GitHub repository.
- The user asks to open, update, review, merge, or triage a PR.
- The user asks to create, label, search, or update issues.
- The task needs GitHub auth setup or troubleshooting.
- The task needs repository statistics or codebase inspection before planning work.

## Workflow Sections

### Authentication
Run `gh auth status` before assuming credentials. Prefer existing authenticated `gh`; only guide token/SSH setup when the command proves auth is missing.

### Repository management
Inspect `git remote -v`, current branch, and working tree before changing remotes or publishing. Avoid force operations unless explicitly requested.

When the user asks to push a small scoped change to a protected/shared branch (for example `main`) while the current checkout is on an unrelated feature branch, do **not** rely on project ship scripts that stage everything or push the current branch. Instead: fetch the target branch, create/switch to a temporary branch from `origin/<target>`, add only the intended paths explicitly, commit, and push `HEAD:<target>`. Verify with `git ls-remote origin refs/heads/<target>` and `git log` that the remote branch points at the intended commit.

### Issues
Fetch existing labels/milestones/assignees before creating structured issues. Use templates from preserved package references when applicable.

### Pull requests and CI
Create PRs from a clean branch, include tested changes, watch checks when requested, and report exact URLs/statuses. For failed checks, fetch logs before guessing.

### Code review
Review the diff, not just file names. Separate blocking correctness/security issues from nits. If posting comments, verify the API response or resulting PR comments.

### Codebase inspection
Use lightweight metrics to orient planning, but do not treat LOC counts as quality judgments.

## Package References

The former focused skills are retained under `references/original-skills/` with their templates, scripts, and detailed recipes intact.

## Verification Checklist

- [ ] Repository and auth state checked.
- [ ] Working tree status known before edits.
- [ ] GitHub side effects verified by URL, ID, or command output.
- [ ] CI/review claims backed by fetched status/logs.
