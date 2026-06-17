---
name: ai-media-production
description: "Use when producing, searching, analyzing, or transforming media: YouTube content, GIFs, music prompts, HeartMuLa generation, spectrogram analysis, ASCII video, or audio/video creative workflows."
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [media, audio, video, music, youtube, gifs]
    related_skills: []
---

# AI Media Production

## Overview

Use this umbrella for media tasks spanning YouTube transcripts, GIF search, music/song prompting, HeartMuLa generation, audio feature visualization, and stylized video transforms. These workflows share the same pattern: acquire media or prompt, transform/analyze it, verify the output file/URL, and deliver in the requested format.

Original packages are preserved under `references/original-skills/<skill-name>/`.

## When to Use

- The user asks to summarize or repurpose YouTube content.
- The user asks to search/download a GIF.
- The user asks for lyrics, Suno-style prompts, music generation, or HeartMuLa.
- The user asks for spectrograms/audio features or ASCII video transforms.

## Mode Subsections

### YouTube and transcripts
Fetch real transcripts or metadata; if unavailable, say so rather than inventing content.

### GIF search
Search/download from the configured provider and verify the resulting file or URL.

### Songwriting and music generation
Separate lyrics, style tags, arrangement notes, and generation-provider constraints.

### Audio analysis
Produce actual spectrogram/features from the input file and deliver the artifact.

### Stylized video
Treat ASCII/video transforms as media pipelines: inspect input, choose resolution/effects, render, and verify playback.

## Verification Checklist

- [ ] Input media exists or source URL was fetched.
- [ ] Output artifact path/URL verified.
- [ ] Provider/API failures reported honestly.
- [ ] Original detailed recipes consulted from references when needed.
