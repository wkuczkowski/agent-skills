---
name: writing-for-agents
description: Holds the mechanics of a finished skill and instruction file in this collection (layout, frontmatter, invocation flags, what each harness loads, how a skill is proven to fire). Use when creating or editing a skill, AGENTS.md or CLAUDE.md, when deciding whether a skill is model-invocable or user-only, or when a skill fails to fire on its trigger.
metadata:
  upstream: mattpocock/skills skills/productivity/writing-for-agents
  upstream-commit: "321658273cb1d20b76026717d027d505790106d4"
  adopted: "2026-09-06"
---

# Writing for agents

The register is the one in the global instructions (`global/AGENTS.md`, sections "The user" and "How the work tends to go"): facts the model cannot guess and observations about what has worked, in English, with imperatives only where a wrong guess costs data, security, an irreversible action or a decision the user made. What follows is the mechanics, which the harnesses fix and the model cannot look up: [references/harnesses.md](references/harnesses.md) for what Claude Code and Codex load and how to test it, [references/models.md](references/models.md) for how Fable 5.1 and Astra react to instruction text.

## Descriptions

- Third person: what it does, then when it applies, key use case first. One trigger per distinct branch.
- Under 1,024 characters and far shorter in practice; every character sits in context on every turn and the whole Codex catalog has 8,000.
- The description decides whether the skill is reached at all; when a skill does not fire, the description is the first thing to change.

## Skills

- Layout: `skills/<name>/SKILL.md`, optional `references/` (one level deep), `scripts/`, `assets/`, `agents/openai.yaml`.
- Frontmatter `name` equals the directory: lowercase letters, digits, hyphens, at most 64 characters. `description` is required. `metadata:` carries `upstream`, `upstream-commit` and `adopted` for adopted skills. Claude-only fields (`context`, `paths`, `hooks`, `allowed-tools`, `when_to_use`) are ignored by Codex.
- Model-invocable is the default: no invocation flag, and `agents/openai.yaml` with an `interface` block only (`display_name`, `short_description` of 25 to 64 characters, `default_prompt` mentioning `$<name>`).
- User-only needs both `disable-model-invocation: true` in the frontmatter and `policy.allow_implicit_invocation: false` in `agents/openai.yaml`; Codex ignores the frontmatter flag. It fits a workflow with side effects that only a human should start.
- `SKILL.md` is a real file; Codex skips a symlinked one silently. The skill directory and `references/` may be symlinks.
- Astra stalls on a skill that could conflict with the user's request unless the skill says the user's instructions take precedence.

## Global and project instructions

- Claude Code reads `CLAUDE.md`, Codex reads `AGENTS.md` from the repo root down to the working directory, 32 KiB combined. A symlink from one name to the other gives both harnesses the same file; Claude Code's `@AGENTS.md` import works for Claude Code only, and Codex reads it as literal text.
- `@imports` load every session, so they organise and save nothing.
- Instruction files are advisory. A rule that must hold with zero exceptions is a hook.

## A skill is done when

For a new skill or one whose description or body changed materially; a small edit needs items 1 and 2. The user may waive any item.

1. `claude plugin validate --strict skills` passes; for a private skill, run it on a scratch `skills/` holding a copy. It accepts only a directory named `skills` (anything else fails with "No manifest found") and ignores `name` and unknown fields, so the frontmatter is checked by hand against the rules above.
2. `manifest.yaml` has the entry (`kind: own`; for adoptions also `adopted: true` and `upstream: <owner/repo> <path>/SKILL.md@<sha>`), `bin/link` has run, and `bin/link check` is clean.
3. One headless run per harness (skills `claude-headless` and `codex-headless`) with a prompt matching the description shows the skill listed and invoked. Evidence is the harness record (Claude Code: the `skill_listing` attachment and a Skill tool call in the transcript; Codex: `<skills_instructions>` in the rollout and a read of `SKILL.md`), never the model's self-report.
4. It agrees with what loads beside it: the descriptions of skills sharing its triggers (`bin/link list <harness>`) and the global instructions; every path, flag and command it names exists.
