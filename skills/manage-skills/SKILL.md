---
name: manage-skills
description: Manages the user's skill collection in the agent-skills repo. Lists which skills Claude Code or Codex sees, adds vendor skills, writes new own skills, adopts a vendor skill, retires skills, edits the manifest, and runs the weekly or monthly review. Use when the user asks what skills a harness sees, wants to add, adopt, retire or review skills, or wants the manifest changed.
---

# Manage skills

The collection lives in `/home/wkuczkowski/projects/TOOLS/skills` (below: the repo), and every `bin/*` and `npx skills` command runs from there. The repo's `AGENTS.md` describes how it works and `GLOSSARY.md` its vocabulary. How a skill is written, and what makes one finished, is in the `writing-for-agents` skill. The user's instructions take precedence over this skill.

## Facts about the repo

- `manifest.yaml` says what exists and which harness sees it. `bin/link` projects it onto `~/.claude/skills` and `~/.agents/skills`, and `bin/link check` reports drift; a clean check reports 0 findings, and `~/.claude/skills/synced` (the desktop app's account skills) is exempt. Both run after any change to the manifest, `skills/`, `private/` or `.agents/skills/`.
- `bin/link list <claude-code|codex>` prints what a harness sees. A Codex session started inside the repo also sees every vendor skill through the project `.agents/skills` root, whatever the manifest says.
- `bin/usage` counts invocations from transcripts (`--write` updates `usage.yaml`, `--unused` lists retirement candidates), and `bin/upstream` reports upstream changes per skill (`--diff <name>` shows one).
- Vendor skills under `.agents/skills/` are verbatim copies that `npx skills update` overwrites. A vendor skill is added with `npx skills add <owner/repo> -a codex -y -s <name>` and a manifest entry `kind: vendor`, `upstream: <source> <skillPath>` copied from `skills-lock.json`.
- `skills/` is public on GitHub, `private/` stays on this machine; `.gitignore` is a whitelist.
- The user browses the collection on a private Artifact page, https://claude.ai/artifact/DKsxYM2rxYmuYjbqbMcZak, a snapshot that does not update itself. `bin/catalog` writes `reports/skills-catalog.html`, which Claude Code republishes to that URL after a change to skills, the manifest or `global/AGENTS.md`; Codex and headless runs cannot publish and say so. The page embeds `global/AGENTS.md`, so it stays out of git and is never shared publicly.

## Lessons

- `npx skills remove` also deletes `skills/<name>` when an own directory of that name exists (2026-09-06). A vendor skill leaves through `bin/unvendor <name>`, which removes only `.agents/skills/<name>` and its lock entry.
- `npx skills check` reinstalls vendor skills as a side effect (2026-10-05); `bin/upstream` reports changes without touching anything.
- Retiring a skill, deleting files and changing skill content are the user's decisions; a review proposes and does not apply.

## Operations

- **Adopting** a vendor skill turns it into an own one under `skills/<same name>`: [references/adopt.md](references/adopt.md).
- **Retiring**: vendor with `bin/unvendor`, own with `git rm -r skills/<name>` (or deleting `private/<name>`), then the manifest entry goes and the projections are rebuilt.
- **The weekly or monthly review**: [references/review.md](references/review.md).
