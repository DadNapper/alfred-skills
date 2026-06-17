---
name: productivity-workflows
description: "Use when operating productivity systems: Airtable, Google Workspace, Notion, maps/geocoding, PDFs/OCR, PowerPoint, Teams meeting pipelines, or similar document/spreadsheet/collaboration workflows."
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [productivity, docs, spreadsheets, pdf, notion, google-workspace]
    related_skills: []
---

# Productivity Workflows

## Overview

Use this umbrella for document, database, spreadsheet, presentation, mapping, OCR/PDF, and meeting-summary operations. These tasks share credential checks, schema discovery, non-destructive edits, and verification by reading the created or modified object back.

Original packages are preserved under `references/original-skills/<skill-name>/`.

## When to Use

- The user asks for Airtable, Google Workspace, Notion, Linear, maps/geocoding, PDFs/OCR, PowerPoint, or Teams meeting pipeline work.
- The task creates or edits external productivity artifacts.
- The task needs API-backed document/database manipulation with verification.

## Mode Subsections

### Tables, databases, and project tools
Discover schema before writes; use IDs rather than display names when available.

### Docs, mail, calendars, and drives
Check credentials/scopes and verify by reading the target object after creation or edit.

### PDFs, OCR, and presentations
Preserve originals, produce new files when possible, and inspect output artifacts.

### Maps and geocoding
Report source, timestamp, coordinates/routes, and uncertainty.

### Meeting pipelines
Treat transcripts and summaries as sensitive; verify the pipeline output location.

## Verification Checklist

- [ ] Credentials and target object identified.
- [ ] Schema/current state read before mutation.
- [ ] Output object/file read back or inspected.
- [ ] Side effects summarized with IDs/URLs/paths.
