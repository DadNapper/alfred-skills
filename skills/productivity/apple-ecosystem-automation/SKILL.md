---
name: apple-ecosystem-automation
description: Use when working across Apple Notes, Reminders, iMessage, Find My, and macOS GUI automation. Provides a class-level workflow with app-specific subsections and keeps original narrow skills as reference packages.
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [apple, macos, productivity, automation]
    related_skills: []
---

# Apple Ecosystem Automation

## Overview

Use this umbrella for Apple-local productivity and device tasks: Notes, Reminders, Messages/iMessage, Find My, and macOS GUI automation. Prefer a single Apple skill because these tasks share the same constraints: user privacy, local app state, macOS-only tooling, AppleScript/Shortcuts/GUI fallbacks, and verification by reading the resulting app state.

Original source packages are preserved under `references/original-skills/<skill-name>/` when present.

## When to Use

- The user asks to create, search, or update Apple Notes.
- The user asks to create or inspect Reminders.
- The user asks to send or inspect iMessage/Messages content.
- The user asks about Find My item/person/device location state.
- The task requires operating the macOS GUI or AppleScript safely.

## App-Specific Notes

### Notes
Treat Notes work as local state modification. Search/read before overwriting, preserve folder/account context, and verify by reading the target note after mutation.

### Reminders
Clarify due dates only when ambiguity changes the reminder. Otherwise use the obvious default list and verify the reminder exists with the intended title/date.

### iMessage
Sending messages is an external side effect: resolve the recipient carefully and report exactly what was sent and to whom. Do not fabricate delivery state.

### Find My
Location data can be stale and privacy-sensitive. Report timestamps and confidence, not just coordinates.

### macOS computer use
Prefer deterministic APIs (AppleScript, Shortcuts, SQLite/CLI where safe) before visual GUI driving. Use GUI automation only when the app has no reliable API.

## Package References

See `references/original-skills/` for the preserved narrow skill packages and any app-specific command details.

## Verification Checklist

- [ ] Confirm platform support before invoking macOS-only commands.
- [ ] Read current state before destructive edits.
- [ ] Verify final state from the app or backing store.
- [ ] Report side effects plainly.
