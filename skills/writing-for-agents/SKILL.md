---
name: writing-for-agents
description: Holds what earns a line in a skill or instruction file in this collection, and the mechanics of a finished one (layout, frontmatter, invocation flags, what each harness loads, how a skill is proven to fire). Use when creating or editing a skill or its description, AGENTS.md or CLAUDE.md, when deciding whether a skill is model-invocable or user-only, or when a skill fails to fire on its trigger.
metadata:
  upstream: mattpocock/skills skills/productivity/writing-for-agents
  upstream-commit: "321658273cb1d20b76026717d027d505790106d4"
  adopted: "2026-09-06"
---

# Writing for agents

## What earns a line

The models these files are written for are capable and change every few weeks, and a line written for one of them ages with it. A line earns its place when removing it would change what an agent decides. In practice that has been:

- **Facts the agent cannot find on its own**: paths, commands, URLs, how this machine and repo are set up, fields in a format nobody documents. Dated, with what they were checked against.
- **Lessons from what went wrong**, dated: "`npx skills remove` also deletes `skills/<name>`" (2026-09-06). A lesson ages slowly and does not narrow the agent's options.
- **What the user dislikes or has rejected.** What he likes is riskier to write down: an agent that reads it may stop there instead of proposing something better, and he wants better options proposed (`global/AGENTS.md`, "The user").
- **The goal and what done looks like**, rather than the steps to get there; the method is the agent's. Steps belong in a file only where their order is fixed and a wrong order breaks something (a sequence of repo commands, a data or security boundary, an irreversible action).

How the agent talks to the user (the shape of a question, a report, a reply) is left to the model and the harness, which keep adding their own interactive ways to ask. A file can name the aim, a decision the user can make easily, in his language, and leave the form open.

Imperatives sit only where a wrong guess costs data, security, an irreversible action or a decision the user made; everywhere else the line is a fact or an observation. Files and skills are in English. A skill that could conflict with the user's request says that his instructions take precedence; without that line, GPT-6 Astra stalled on such skills (2026-09).

The rest of this skill is mechanics the harnesses fix: [references/harnesses.md](references/harnesses.md) for what Claude Code and Codex load and how to test it.

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

## Global and project instructions

- Codex reads `AGENTS.md` from the repo root down to the working directory, 32 KiB combined. Claude Code 2.1.285 reads `CLAUDE.md` and, by default (`instructionFiles: claude-md-or-agents-md`), loads a directory's `AGENTS.md` where that directory has no `CLAUDE.md`; so one `AGENTS.md` and no `CLAUDE.md` serves both harnesses, which is what the user settled on 2026-09-22. Claude Code's `@AGENTS.md` import works for Claude Code only, and Codex reads it as literal text.
- `@imports` load every session, so they organise and save nothing.
- Instruction files are advisory. A rule that must hold with zero exceptions is a hook.

## A skill is done when

For a new skill or one whose description or body changed materially; a small edit needs items 1 and 2. The user may waive any item.

1. `claude plugin validate --strict skills` passes; for a private skill, run it on a scratch `skills/` holding a copy. It accepts only a directory named `skills` (anything else fails with "No manifest found") and ignores `name` and unknown fields, so the frontmatter is checked by hand against the rules above.
2. `manifest.yaml` has the entry (`kind: own`; for adoptions also `adopted: true` and `upstream: <owner/repo> <path>/SKILL.md@<sha>`), `bin/link` has run, and `bin/link check` is clean.
3. One headless run per harness (skills `claude-headless` and `codex-headless`) with a prompt matching the description shows the skill listed and invoked. Evidence is the harness record (Claude Code: the `skill_listing` attachment and a Skill tool call in the transcript; Codex: `<skills_instructions>` in the rollout and a read of `SKILL.md`), never the model's self-report.
4. It agrees with what loads beside it: the descriptions of skills sharing its triggers (`bin/link list <harness>`) and the global instructions; every path, flag and command it names exists.
