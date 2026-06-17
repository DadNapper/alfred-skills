---
name: ai-creative-visuals
description: "Use when creating creative artifacts: diagrams, HTML mockups, design systems, p5.js sketches, Excalidraw drawings, Pretext demos, infographics, comics, pixel art, text humanization, or other creative-generation workflows."
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [creative, design, diagrams, html, p5js, excalidraw, infographic]
    related_skills: []
---

# AI Creative Visuals

## Overview

Use this umbrella for visual creative work. The former narrow skills are better treated as modes inside one class: choose the artifact type, select a style system, generate or edit the asset, then verify visually or by opening/exporting the file.

Original packages, including templates and scripts, are preserved under `references/original-skills/<skill-name>/`.

## When to Use

- The user asks for a diagram, architecture visualization, or Excalidraw-style sketch.
- The user asks for a landing page, design mockup, prototype, or design-system-inspired HTML.
- The user asks for p5.js/generative art, Pretext demos, pixel art, infographics, comics, or illustration prompts.
- The task needs style/palette/layout guidance for visual generation.

## Mode Subsections

### Diagrams and whiteboard artifacts
Use architecture-diagram or Excalidraw references for structure, labels, dark/light mode, and export formats.

### HTML mockups and design systems
Use sketch, claude-design, pretext, popular-web-designs, and design-md references for polished web artifacts, design tokens, and comparison variants.

### Creative writing polish
Use humanizer references when the requested artifact is prose rather than imagery: remove generic AI cadence, preserve the user's voice, and keep edits grounded in the source text.

### Generative/interactive visuals
Use p5.js and TouchDesigner references for animation loops, WebGL, interaction, and export pipelines.

### Knowledge visuals and illustration prompts
Use Baoyu infographic/comic/article references for layout, narrative, palette, and prompt construction.

### Pixel and ASCII styles
Use pixel-art/ascii-art/ascii-video references when the visual language is intentionally low-resolution or terminal-inspired.

## Verification Checklist

- [ ] Artifact type and target medium selected.
- [ ] Style/palette/layout constraints explicit.
- [ ] Generated file opened, rendered, or exported when possible.
- [ ] Source package references consulted for tool-specific commands.
