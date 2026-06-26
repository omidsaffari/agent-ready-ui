# AGENTS.md

This repository ships `agent-ready-ui`, a small Agent Skills artifact for making
web interfaces easier for browser agents and Playwright tests to operate.

## Editing Principles

- Keep the skill focused on one job: improve UI affordances so agents can click,
  type, wait, and verify without brittle selectors.
- Prefer accessible role/name, label, and state contracts over CSS selectors,
  child indexes, generated class names, or layout-dependent test hooks.
- Preserve the product UI unless a visible label, state, or message is needed
  for both humans and agents to understand the interface.
- Avoid hidden bot-only controls or instructions that make the UI misleading.
- Keep examples copyable and small enough to apply to a real React, Next.js, or
  component-library codebase.

## Files

- `SKILL.md` is the executable instruction surface for Claude Code, Codex, and
  Agent Skills-compatible tools.
- `README.md` is the public landing page and install guide.
- `examples/` contains concrete before/after flows.
- `references/` contains reusable templates and implementation guidance.
- `assets/` contains the demo GIF and Open Graph image.

## Release Checks

Before publishing a change:

- Run `npx -y markdownlint-cli2 "**/*.md" "#node_modules"`.
- Confirm `SKILL.md` still has `name` and `description` frontmatter.
- Keep `README.md` installation commands valid for both Claude Code and Codex.
- Update `CHANGELOG.md` for user-visible changes.
