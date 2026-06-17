---
name: system-and-service-operations
description: "Use when maintaining Linux workstations, supervising services, configuring webhooks, operating MCP servers, smart-home bridges, or other local service integrations."
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [devops, linux, services, webhooks, mcp, maintenance]
    related_skills: []
---

# System and Service Operations

## Overview

Use this umbrella for local system/service operations: workstation maintenance, service supervision, webhook subscriptions, MCP server configuration, smart-home bridges, and container process trees. The shared class workflow is inspect current state, make conservative changes, and verify with live service status/logs.

Original packages are preserved under `references/original-skills/<skill-name>/`.

## When to Use

- The user asks to audit or maintain a Linux workstation.
- The user asks to configure webhooks, MCP servers, or service integrations.
- The user asks to debug supervision/s6/container service behavior.
- The task touches smart-home bridges or local daemons.

## Mode Subsections

### Workstation maintenance
Prefer conservative, reversible cron/systemd tasks; inspect before cleanup.

### Service supervision
Read process trees, supervisor configs, and logs before editing restart behavior.

### Webhooks and MCP
Verify endpoints, credentials, event scopes, and test deliveries/tool registration.

### Smart-home services
Confirm bridge/device identity and avoid unsafe physical-world side effects.

## Verification Checklist

- [ ] Current service/system state inspected.
- [ ] Config changes backed up or reversible.
- [ ] Logs/status/health endpoints verified.
- [ ] Side effects and rollback path reported.
