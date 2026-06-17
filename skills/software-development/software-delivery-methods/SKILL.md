---
name: software-delivery-methods
description: "Use when planning, debugging, testing, reviewing, simplifying, or spiking software changes. Consolidates code-delivery method skills while leaving protected slash-command entry points untouched."
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [software-development, debugging, testing, review, planning]
    related_skills: [plan]
---

# Software Delivery Methods

## Overview

Use this umbrella for general software-development methods: systematic debugging, test-driven development, requesting reviews, simplification, spikes, and debugger-driven investigation. These are stages of one delivery loop rather than separate library concepts.

Protected built-ins such as `plan` are not absorbed. Original packages are preserved under `references/original-skills/<skill-name>/`.

## When to Use

- The user asks to debug a failing system or trace root cause.
- The user asks for TDD, tests before implementation, or regression coverage.
- The user asks to simplify/refactor code or request a review.
- The user asks for a spike/prototype to validate an approach.
- The task needs Python debugpy or Node inspector workflows.

## Method Subsections

### Systematic debugging
Reproduce, localize, explain root cause, then fix. Do not patch symptoms blindly.

### Test-driven development
Write or identify a failing test first, make it pass minimally, then refactor.

### Review and simplification
Use automated checks plus human diff review. Prefer smaller clearer code over cleverness.

### Spikes
Time-box experiments, isolate throwaway code, and capture the decision/lesson.

### Static source-of-truth audits
For code/data consistency audits (localization CSVs, manifests, generated tables), write a deterministic extractor/checker, compare emitted runtime sources against the source-of-truth file, fix both code shape and data rows, then re-run the checker to zero misses. For Roblox localization audits specifically, use `references/roblox-localization-source-audits.md`.

**Roblox localization pitfall:** when adding player-facing strings that are routed through `LocalizationManager` / `NotificationManager`, update both the code path and the upload source CSV (`Localization/Roblox_Localization_Source_Upload.csv`), not just the manifest docs. Verify by parsing the CSV with `csv.DictReader` and asserting every new `sourceText` / notification string is present exactly; quote CSV rows containing commas or multiline text.

### Roblox live event modifiers
When diversifying or adding timed gameplay events in Roblox, prefer a pure shared config module plus server-rolled replicated attributes, then update every consumer (economy/drop/processing/UI) to read the selected variant. See `references/roblox-live-event-modifiers.md` for the implementation and verification checklist.

### Roblox Experience Config live toggles
When debugging Roblox Experience Config / `ConfigService` feature flags that do not update until Studio or a server restarts, distinguish platform propagation delay from game listener bugs. `ConfigService` is server-only, published/staged changes can take minutes to propagate, and existing clients need a replicated flag listener to refresh bootstrapped UI/tools/world affordances after startup. For instant Studio listener verification, set the server-owned replicated BoolValue directly from the Server console; for config-pipeline verification, use server-side `SetTestingValue` plus `ConfigSnapshot:Refresh()`. See `references/roblox-experience-config-live-updates.md`.

### Roblox live event telemetry
When auditing or adding seasonal-event analytics, prefer server-side Roblox Analytics custom events beside authoritative success paths instead of adding DataStore writes solely for metrics. If legacy DataStore-backed telemetry is harming write budgets, sunset the telemetry storage path explicitly: disable all telemetry flush modes/retries/sampling, replace telemetry DataStore modules with no-op compatibility facades, and remove analytics dedupe writes to player data while keeping gameplay persistence intact. Verify the whole telemetry chain: in-memory model, persistence flags, DataStore serialization payload, admin export/webhook mapping, dashboard fields, and indirect analytics-state writes. Watch for fields that exist in telemetry modules but are omitted from save/export payloads. See `references/roblox-live-event-telemetry.md` for the low-DataStore-pressure checklist, sunset pattern, and metric set.

### Roblox experience launch notifications
When planning Roblox launch or live-ops notification strategy, distinguish Experience Event “Notify Me”, Experience Update Announcements, and the personalized Experience Notifications API. For staged launches, use community channels for soft launch, dashboard Update Announcement for public opted-in experience users, and manual server-authoritative game config for known event start/end times. Avoid custom broad notification schedulers unless per-user targeting is required. See `references/roblox-experience-launch-notifications.md`.

### Roblox spray / graffiti click debugging
When a Roblox tool equips correctly but clicks/taps on invisible or client-revealed placement parts do nothing, debug the input-to-raycast-to-picker chain before changing server placement or persistence. Check `gameProcessed`/GUI blockers, tool listener/proximity state, bound target counts, client `CanQuery` restoration, raycast assist rejection, camera coordinate API choice, `_G` picker early returns, then server RemoteEvent rejection. See `references/roblox-spray-graffiti-debugging.md` for the ordered probe list and verification checklist.

### Roblox Studio MCP debugging / Cursor handoff
When updating Cursor/agent instructions or running Studio-assisted Roblox debugging, cross-check the current Creator Hub Studio MCP docs before locking tool names. Document the full MCP tool surface by category, account for current-vs-legacy aliases (`console_output` vs `get_console_output`, `execute_luau` vs `run_script_in_play_mode`, `mouse_input`/`keyboard_input` vs `user_*`), and include multi-Studio selection plus baseline → fix → verification → cleanup workflow. For temporary diagnostics, consider Roblox structured `LogService` methods with stable templates and small context tables. See `references/roblox-studio-mcp-debugging.md`.

### Debugger workflows
Use debugpy or Node inspector when breakpoints/state inspection beats log guessing.

## Verification Checklist

- [ ] Reproduction or acceptance criteria captured.
- [ ] Tests/checks run with real output.
- [ ] Diff reviewed after changes.
- [ ] Remaining risk or follow-up stated plainly.
