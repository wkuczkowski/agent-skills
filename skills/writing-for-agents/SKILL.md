---
name: writing-for-agents
description: Writes and reviews the documents agents consume, and holds this repo's standard for a finished skill. Use when creating or editing a skill, AGENTS.md, CLAUDE.md, or a doc reached by a pointer; when deciding whether a skill is model-invocable or user-only; or when a skill fails to fire on its trigger.
metadata:
  upstream: mattpocock/skills skills/productivity/writing-for-agents
  upstream-commit: "321658273cb1d20b76026717d027d505790106d4"
  adopted: "2026-09-06"
---

# Writing for agents

Rules for any document an agent reads: a skill, an `AGENTS.md` or `CLAUDE.md`, a file reached by a pointer. The packaging differs; the writing is the same. Read [references/theory.md](references/theory.md) when a rule below is unclear or two rules pull against each other, [references/harnesses.md](references/harnesses.md) for what Claude Code and Codex actually load and how to test it, and [references/models.md](references/models.md) before tuning wording for Fable 5.1 or Astra.

Skills and instruction files are written in English.

## Every document

- Keep a line only when it changes behaviour against the model's default. Delete a failing sentence whole.
- State what to do. The why and the how go behind a pointer.
- Name the target behaviour. A prohibition earns its place only as a guardrail, written next to the positive form.
- A brief instruction steers a behaviour. Enumerate cases only where the cases differ.
- Plain register: "Use X when ...". Emphasis goes on the one line that keeps being skipped, and nowhere else.
- Categorical wording ("always", "never", "must") is for where a wrong guess costs something real: data, security, a destructive action, a decision the user has made. Elsewhere state the intent, or a default the agent may override with judgement; models read intent better each release and carry more memory, so heavy rule sets age badly. Unsure whether a rule is still needed: drop it and watch.
- End each step on a criterion the agent can check. Say what done looks like and what to leave out.
- Inline what every path through the document needs. Push what only some paths reach into a file behind a pointer that names the condition for opening it.
- One meaning lives in one place. The environment (config files, `--help`, the directory tree) is a source too; restate only what the agent cannot look up.
- Replace a spelled-out triad with one strong word the model already knows, and reuse that word wherever the behaviour is meant.
- Reviewing an existing document means running these rules over every sentence and reporting deletions before rewrites.

## Pointers and descriptions

- Third person. What it does, then when it applies, key use case first.
- One trigger per distinct branch; synonyms of the same branch are one trigger.
- Description under 1,024 characters and far shorter in practice: every character sits in context on every turn, and the whole Codex catalog has 8,000.
- The pointer's wording decides whether the target is reached. Sharpen it before inlining the material.

## Skills

- Layout: `skills/<name>/SKILL.md`, optional `references/` (one level deep, a table of contents once a file passes 100 lines), `scripts/`, `assets/`, `agents/openai.yaml`.
- Frontmatter `name` equals the directory: lowercase letters, digits, hyphens, at most 64 characters. `description` is required. `metadata:` carries `upstream` and `adopted` for adopted skills. Claude-only fields (`context`, `paths`, `hooks`, `allowed-tools`, `when_to_use`) are ignored by Codex.
- Body well under 500 lines. Here: rules and routing in `SKILL.md`, procedures and facts in `references/`.
- Model-invocable is the default: no invocation flag, and `agents/openai.yaml` with an `interface` block only (`display_name`, `short_description` of 25 to 64 characters, `default_prompt` mentioning `$<name>`).
- User-only needs both `disable-model-invocation: true` in the frontmatter and `policy.allow_implicit_invocation: false` in `agents/openai.yaml`; Codex ignores the frontmatter flag. Choose it when only a human should start the skill, typically a workflow with side effects.
- `SKILL.md` is a real file; Codex skips a symlinked one silently. The skill directory and `references/` may be symlinks.
- Scripts solve rather than defer: they handle their own errors, and the body says whether to run or read them.
- A skill yields to the user's explicit instructions. Say so in the body when the skill could otherwise stall Astra.

## Global and project instructions

- Under 200 lines. Only always-on facts belong there: commands the agent cannot guess, conventions that differ from defaults, gotchas. A multi-step procedure becomes a skill; a topic used sometimes becomes a plain path pointer.
- `@imports` load every session, so they organise and save nothing.
- Claude Code reads `CLAUDE.md`, not `AGENTS.md`; a `CLAUDE.md` containing `@AGENTS.md` bridges them. Codex reads `AGENTS.md` from the repo root down to the working directory, 32 KiB combined.
- Instruction files are advisory. A rule that must hold with zero exceptions is a hook.
- Line test: would removing it cause a mistake in a session? Otherwise cut it.

## Models

- Fable 5.1: a brief instruction beats an enumeration; drop "CRITICAL: you MUST" emphasis and older prescriptive scaffolding; never ask it to show its reasoning; it batches fewer implied tool calls and may stop before finishing, so state the completion criterion and what to leave out.
- Astra: more sensitive to skill and `AGENTS.md` text, so an unclear or conflicting line stalls it; state that user instructions take precedence; ask for prose where prose is wanted, since it defaults to list-heavy output with recurring phrases; say when to delegate and which checks suffice, since it delegates less and over-tests small changes.

## A skill is done when

1. `claude plugin validate skills/<name>` reports no findings.
2. `manifest.yaml` has the entry (`kind: own`; for adoptions also `adopted: true` and `upstream: <owner/repo> <path>/SKILL.md@<sha>`), `bin/link` has run, and `bin/link check` is clean apart from known findings.
3. One headless run per harness (skills `claude-headless` and `codex-headless`) with a prompt matching the description shows the skill listed and invoked. Evidence is the harness record (Claude Code: the `skill_listing` attachment and a Skill tool call in the session transcript; Codex: `<skills_instructions>` in the rollout and a read of `SKILL.md`), never the model's self-report.
4. It agrees with what loads beside it: read the descriptions of the skills sharing its triggers (`bin/link list <harness>`) and the global instructions (`global/AGENTS.md`), and settle every overlap or contradiction in one of the two places; every path, flag and command it names exists.
