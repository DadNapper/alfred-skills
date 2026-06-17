---
name: mlops-model-workflows
description: "Use when finding, evaluating, serving, modifying, tracking, or operating ML/LLM models across Hugging Face, lm-eval, W&B, llama.cpp, vLLM, obliteration, AudioCraft, or Segment Anything workflows."
version: 1.0.0
author: Hermes Agent
license: MIT
metadata:
  hermes:
    tags: [mlops, llm, inference, evaluation, huggingface, wandb]
    related_skills: []
---

# MLOps Model Workflows

## Overview

Use this umbrella for model operations across discovery, evaluation, serving, experiment tracking, model surgery, and specialized model families. These workflows share environment management, artifact/version discipline, hardware checks, and verification through real commands or generated artifacts.

Original packages are preserved under `references/original-skills/<skill-name>/`.

## When to Use

- The user asks to discover/download/upload models or datasets.
- The user asks to evaluate LLMs, track experiments, or use W&B.
- The user asks to serve models with llama.cpp or vLLM.
- The user asks for abliteration/model surgery, AudioCraft generation, or Segment Anything masks.

## Mode Subsections

### Hub and artifacts
Treat model/dataset IDs, revisions, and local paths as explicit inputs; verify downloads/uploads.

### Evaluation and tracking
Pin benchmark tasks/configs and log exact command lines, seeds, and result files.

### Inference serving
Check hardware, model format, quantization, ports, and health endpoints before declaring a service ready.

### Model surgery and specialized models
Preserve reproducible configs and validate outputs qualitatively and quantitatively.

## Verification Checklist

- [ ] Environment and hardware checked.
- [ ] Model/artifact path or ID verified.
- [ ] Commands run with logged outputs.
- [ ] Generated metrics/files/endpoints inspected.
