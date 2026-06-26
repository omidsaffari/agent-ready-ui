# AGENTS.md — build spec for a skill / CLAUDE.md artifact

This repo is **skill-starter**, the omidsaffari Labs scaffold for shipping ONE
small, forkable **skill** (or a single `CLAUDE.md`) as an authoritative OSS repo.
Codex clones it with `gh repo create omidsaffari/<slug> --template omidsaffari/skill-starter`,
then fills it in per this spec. Read this fully before editing.

## Doctrine (non-negotiable)

- Build the **narrowest perimeter artifact** that makes an existing agent (Claude
  Code / Cursor / ChatGPT) better. **One job per skill.**
- **Never** frame as "AI-powered". Lead with the concrete problem it kills, or
  "the open-source alternative to <incumbent>".
- The metric is **forks / awesome-list inclusion / citations**, not stars. The
  README + the skill quality are the entire product.
- Banned: general agent frameworks, chatbot wrappers, Lovable/v0/bolt clones.

## What you edit vs leave

- **Edit:** `SKILL.md` (the skill itself), `README.md` (the landing page),
  optional `scripts/` `references/` `assets/`, and the repo name/description.
- **Keep:** the `name` + `description` frontmatter keys in `SKILL.md` (CI checks
  them), the `.github/workflows/ci.yml` gate, the MIT `LICENSE` (set the year +
  "Omid Saffari" — already done).
- A `CLAUDE.md` artifact instead of a skill? Replace `SKILL.md` with a single
  top-level `CLAUDE.md`, drop the skill install section, and keep everything else.

## Fill `SKILL.md`

- `name`: lowercase-kebab, matches the repo slug.
- `description`: front-load trigger words; say when it SHOULD and SHOULD NOT fire.
  Progressive disclosure shows only name+description until the skill loads, and
  long ones get shortened — so the first clause must carry the trigger.
- Body: imperative steps, explicit inputs/outputs. Instructions over scripts
  unless deterministic behavior or external tooling is required.

## Fill `README.md` (the conversion surface)

Above-the-fold order, in this order: **title → one-line problem statement →
demo GIF (`assets/demo.gif`, the single best 5–15s moment) → one badge row →
install (Claude Code + Codex)**. Keep it skimmable; deep docs go in `references/`.

## Ship checklist (do every release)

0. **First fill step:** `rm .skill-starter-template` (this switches on the strict
   release CI gates and signals all placeholders must now be replaced).
1. `name`/`description` valid; CI green.
2. `README.md` filled; `assets/demo.gif` present; `assets/og.png` (1280×640,
   solid bg, <1MB) set as the repo's social-preview image.
3. Repo is a **template repo**; MIT `LICENSE`; 10–20 exact-match **Topics**
   (e.g. `skill`, `claude-code`, `codex`, `agent`, + the niche).
4. A GitHub **Release** + a one-line `CHANGELOG` entry.
5. Distribution: submit to the relevant **awesome-list(s)**; draft the build-log
   for the Labs catalog; one fresh **Show HN** (Sunday ~noon UTC, problem-framed
   title, repo in URL, founder comment in 5 min).
