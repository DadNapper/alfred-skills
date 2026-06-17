---
name: research-intelligence-workflows
description: "Use when discovering, monitoring, synthesizing, or writing research and market intelligence from arXiv, blogs/RSS, LLM wikis, Polymarket, or academic paper-writing workflows."
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [research, arxiv, papers, monitoring, synthesis]
    related_skills: []
---

# Research Intelligence Workflows

## Overview

Use this umbrella for research discovery, literature monitoring, market/probability intelligence, knowledge-base querying, and academic paper writing. The common class workflow is source acquisition, provenance preservation, synthesis, and clearly separated claims versus speculation.

Original packages are preserved under `references/original-skills/<skill-name>/`.

## When to Use

- The user asks to find papers, authors, datasets, or related work.
- The user asks to monitor blogs/RSS or build a watch workflow.
- The user asks to query a local LLM wiki or synthesize a knowledge base.
- The user asks for Polymarket market data or academic paper drafting.

## Mode Subsections

### Paper discovery
Use primary sources when possible, preserve titles/authors/URLs, and avoid inventing citations.

### Monitoring
Make polling state explicit and keep recurring jobs self-contained.

### Knowledge-base synthesis
Separate extracted notes, generated summaries, and user-facing conclusions.

### Market intelligence
Report timestamps, sources, prices/probabilities, and uncertainty.

### Paper writing
Use templates and venue constraints from preserved references; verify generated LaTeX/build artifacts.

## Verification Checklist

- [ ] Source URLs/IDs captured.
- [ ] Dates and freshness reported.
- [ ] Generated claims trace to sources.
- [ ] Output format matches the user's requested artifact.
