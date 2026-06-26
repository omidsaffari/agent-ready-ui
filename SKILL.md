---
name: skill-name
description: >-
  One sentence that says exactly WHEN this skill should and should NOT trigger.
  Front-load the trigger words — Codex/Claude see only name + description until
  they decide to load this file, and long descriptions get shortened. One job.
  Example: "Audit a Next.js repo for N+1 database queries when the user asks to
  find slow or repeated DB access. Not for general performance or non-DB code."
---

# Skill Title

## Overview

One to three sentences: what this skill does and the single concrete problem it
solves. No "AI-powered" framing — name the problem and the outcome.

## When to use

- Trigger when: <explicit situations>
- Do NOT trigger when: <explicit boundaries — keep the scope narrow>

## Workflow

### 1) <First step>

Imperative instructions with explicit inputs and outputs. Prefer instructions
over scripts unless deterministic behavior or external tooling is required.

### 2) <Second step>

Keep steps small and verifiable.

### 3) Report

State exactly what to output back to the user and in what shape.

<!--
  This is the open agentskills.io format — the SAME SKILL.md works in both
  Claude Code (.claude/skills/<name>/) and Codex (.agents/skills/<name>/).
  Keep it to ONE job. Optional siblings: scripts/  references/  assets/  agents/openai.yaml
  Frozen: the `name` + `description` frontmatter keys must remain present (CI checks them).
-->
