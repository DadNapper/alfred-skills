---
name: messaging-and-social-platforms
description: "Use when interacting with external messaging or social platforms such as X/Twitter, Yuanbao, Telegram-specific project operations, or other chat/social workflows."
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [social, messaging, x, yuanbao, telegram]
    related_skills: []
---

# Messaging and Social Platforms

## Overview

Use this umbrella for tasks that post, read, search, or operate within social/messaging platforms. These workflows share recipient/channel resolution, privacy checks, rate-limit awareness, and verification of external side effects.

Original packages are preserved under `references/original-skills/<skill-name>/`.

## When to Use

- The user asks to post/search/DM on X/Twitter.
- The user asks to operate in Yuanbao groups or mention users.
- The user asks for Telegram-topic-specific project operations.
- The task writes to an external social or messaging channel.

## Platform Subsections

### X/Twitter
Resolve account/auth state, keep post text exact, and verify IDs/URLs after posting.

### Yuanbao
Resolve group/user identifiers before mentions and report the exact target.

### Telegram project operations
Respect topic conventions, project-specific references, and side-effect verification.

## Verification Checklist

- [ ] Target channel/account resolved.
- [ ] Message/post content shown exactly.
- [ ] External side effect verified by ID, URL, or readback.
- [ ] Privacy-sensitive data handled conservatively.
