# Harness mechanics

What Claude Code and Codex actually load, when, and with which limits, plus the repo's own standard for a finished skill. Sources: `research/claude-code-skills-and-fable-2026-09-06.md` (Claude Code 2.1.263), `research/codex-skills-and-astra-2026-09-06.md` (codex-cli 0.153.4), `research/skill-loading-test-2026-09-06.md` (empirical loading test on this machine), `research/skills-cli-and-cursor-2026-09-06.md` (`npx skills` 1.5.23). Versions move; when a claim matters, re-check the newest research note or the harness itself.

Contents: [What loads when](#what-loads-when), [Frontmatter](#frontmatter), [agents/openai.yaml](#agentsopenaiyaml), [Invocation modes](#invocation-modes), [Discovery and symlinks](#discovery-and-symlinks), [Instruction files](#instruction-files), [Validation](#validation), [Headless test](#headless-test), [Repo standard](#repo-standard).

## What loads when

Claude Code (`research/claude-code-skills-and-fable-2026-09-06.md`, section 1.5):

- Session start injects a listing of every skill's name and description (plus `when_to_use`), as a system reminder. Each entry is cut at 1,536 characters. The whole listing has a budget of 1% of the context window; when it overflows, descriptions are dropped starting with the least-invoked skills. Every entry costs on every turn.
- On invocation the rendered `SKILL.md` enters the conversation as one message and stays for later turns; the file is not re-read. After auto-compaction the first 5,000 tokens of each invoked skill are re-attached, 25,000 combined.
- Files under `references/` cost nothing until the model reads them.

Codex (`research/codex-skills-and-astra-2026-09-06.md`, sections 1.5 and 1.6):

- Session start injects a `## Skills` catalog into the developer message: name, description and path per skill, capped at 2% of the context window or 8,000 characters (`skills.max_context_tokens`, at most 10,000 tokens). Descriptions are shortened first, then skills are omitted with a warning. Each description is cut at 1,024 characters in the catalog.
- The catalog's own rules: when the user names a skill (`$name`) or the task clearly matches a description, the model must use that skill for the turn, must read its `SKILL.md` completely before acting, and must not delegate that reading to a subagent. Skills do not carry across turns unless mentioned again.
- Codex records an implicit invocation when a command reads a skill's `SKILL.md` or runs a file under its `scripts/`.

## Frontmatter

The Agent Skills spec (agentskills.io, linked by both vendors) defines `name`, `description`, `license`, `compatibility`, `metadata`, `allowed-tools`. `name`: 1 to 64 characters, lowercase letters, digits and hyphens, no leading, trailing or double hyphen, equal to the directory name. `description`: 1 to 1,024 characters. Body recommendation: under 500 lines, under 5,000 tokens; references one level deep.

Claude Code accepts 14 more fields (`research/claude-code-skills-and-fable-2026-09-06.md`, section 1.2): `when_to_use` (extra trigger text, counts toward the 1,536 cap), `argument-hint`, `arguments`, `disable-model-invocation`, `user-invocable`, `disallowed-tools`, `model`, `effort`, `context: fork` (runs the skill in a subagent), `agent`, `background`, `hooks`, `paths` (auto-load only when working on matching files), `shell`. Any of these makes a claude.ai upload or the Skills API reject the file. `metadata` is free-form and not acted on. Body substitutions: `$ARGUMENTS`, `$N`, `${CLAUDE_SKILL_DIR}`, `${CLAUDE_PROJECT_DIR}`; inline `` !`command` `` runs before the body is sent.

Codex reads `name`, `description` and `metadata.short-description`; everything else is ignored (`research/codex-skills-and-astra-2026-09-06.md`, section 1.1). Its bundled validator allows only the spec fields and rejects `<` or `>` in the description, so a description stays free of angle brackets.

## agents/openai.yaml

Read by the Codex harness, never by the model (`research/codex-skills-and-astra-2026-09-06.md`, section 1.2):

```yaml
interface:
  display_name: "Human-facing name"
  short_description: "25 to 64 characters, shown in the picker"
  default_prompt: "Use $skill-name to ..."
policy:
  allow_implicit_invocation: true   # default; false hides the skill from the catalog
dependencies:
  tools:                            # MCP only
    - type: mcp
      value: github
```

An `interface` block alone is the shape for a model-invocable skill; add `policy` only for user-only. Claude Code never reads this file.

## Invocation modes

| Intent | Claude Code (`SKILL.md`) | Codex (`agents/openai.yaml`) | In context |
|---|---|---|---|
| Model and human may start it | no flag | no `policy` | description always |
| Human only | `disable-model-invocation: true` | `policy.allow_implicit_invocation: false` | description absent |
| Model only, hidden from the `/` menu | `user-invocable: false` | no equivalent | description always |

Each harness reads only its own flag (`research/skill-loading-test-2026-09-06.md`, summary table): Codex lists a skill whose frontmatter says `disable-model-invocation: true` when no `openai.yaml` policy hides it, and Claude Code lists a skill whose `openai.yaml` says `false`. The frontmatter is this repo's single truth; `bin/link check` reports a mismatch. `disable-model-invocation: true` also stops Claude Code from preloading the skill into subagents and from running it as a scheduled task, and blocks the model if it tries to call it anyway. A Codex skill hidden by policy can still be started with `$name`.

## Discovery and symlinks

Claude Code reads `~/.claude/skills/`, `.claude/skills/` from the working directory up to the repo root (nested ones load on first touch), plugin `skills/`, `--add-dir` directories and the managed directory. It does not read `.agents/skills` or `~/.agents/skills`. It follows directory symlinks and file symlinks, and watches skill directories for changes (`research/claude-code-skills-and-fable-2026-09-06.md`, section 1.4; `research/skill-loading-test-2026-09-06.md`).

Codex reads `.agents/skills` in every directory from the working directory up to the repo root, `<repo>/.codex/skills`, `~/.agents/skills`, `~/.codex/skills` (deprecated in source, still scanned), `/etc/codex/skills` and its bundled `.system` skills. It follows directory symlinks outside `.system`. It skips a skill whose `SKILL.md` is a symlink, silently, whatever the policy; a symlinked `references/` next to a real `SKILL.md` loads (`research/codex-skills-and-astra-2026-09-06.md`, sections 1.3 and 1.4; `research/skill-loading-test-2026-09-06.md`). A Codex session started inside this repo therefore sees every vendor skill under `.agents/skills`, whatever the manifest says (`docs/adr/0001-...`).

Cursor reads `.agents/skills`, `.cursor/skills`, their `~` variants and, for compatibility, the Claude and Codex directories; it is out of scope here (`research/skills-cli-and-cursor-2026-09-06.md`, section B).

## Instruction files

Claude Code (`research/claude-code-skills-and-fable-2026-09-06.md`, section 2.2):

- Load order: managed policy, `~/.claude/CLAUDE.md`, `./CLAUDE.md` or `./.claude/CLAUDE.md`, `./CLAUDE.local.md`, plus `.claude/rules/*.md` (optionally `paths:`-scoped) and auto memory. Delivered as a user message after the system prompt. Target under 200 lines per file; longer files reduce adherence.
- `@path` imports resolve relative to the importing file, recurse four levels, and load at launch; they organise and save nothing. Wrap `@name` in backticks to mention without importing.
- Claude Code reads `CLAUDE.md`, not `AGENTS.md`. A `CLAUDE.md` whose content is `@AGENTS.md` bridges the two; this repo does exactly that.
- Keep: commands the model cannot guess, style rules that differ from defaults, testing and repository etiquette, gotchas, rationale. Cut: anything derivable from the code, standard language conventions, long tutorials, file-by-file descriptions. A multi-step procedure or a topic that matters only sometimes moves to a skill or a path-scoped rule. If one instruction keeps being skipped, emphasise that line alone.
- Instructions are advisory. Enforcement with zero exceptions is a `PreToolUse` hook.

Codex (`research/codex-skills-and-astra-2026-09-06.md`, sections 2.3 and 2.4):

- Global: `~/.codex/AGENTS.override.md` if present, else `~/.codex/AGENTS.md`. Project: one file per directory from the repo root down to the working directory, `AGENTS.override.md` before `AGENTS.md`, concatenated root first so closer files override. Combined cap `project_doc_max_bytes`, 32 KiB. Untrusted projects contribute no project-level file.
- Guidance: the global file holds reusable working agreements, the repo file holds repository expectations; skills hold repeatable workflows; hooks hold mechanical enforcement.

This repo's `global/AGENTS.md` is symlinked as both `~/.claude/CLAUDE.md` and `~/.codex/AGENTS.md`, so one file must satisfy both harnesses: under 200 lines, always-on behaviour only, everything conditional pushed to a skill or a plain path pointer.

## Validation

- `claude plugin validate <dir>` validates the skills in a directory (2.1.233+); `--strict` fails on unrecognised fields and missing metadata. `bin/link check` runs it over `skills/` and `private/`, and additionally checks frontmatter name against directory, description presence and length against both harnesses, invocation-mode mismatches, projection drift and the Codex catalog size.
- `/skill-doctor` in Claude Code (2.1.252+) shows each skill's context cost and usage; `/doctor` estimates the listing's total.
- Codex's `$skill-creator` ships `scripts/quick_validate.py` for a single skill.

## Headless test

The proof that a skill loads and fires is the harness's own record, never the model's self-report; in the loading test Codex misreported three runs out of six (`research/skill-loading-test-2026-09-06.md`, "Self-report reliability"). Run one read-only prompt per harness that matches the description, from a directory outside this repo so the project `.agents/skills` root does not mask the manifest.

Claude Code, through the `claude-headless` skill with session persistence left on: the transcript `~/.claude/projects/<cwd-slug>/<session>.jsonl` carries a `skill_listing` attachment (`names` array) for loading, and a `Skill` tool call for invocation.

Codex, through the `codex-headless` skill with `codex exec --json ... </dev/null` (an open stdin stalls it): the rollout `~/.codex/sessions/<yyyy>/<mm>/<dd>/rollout-*.jsonl` carries the `<skills_instructions>` block in the first developer message for loading, and a command reading the skill's `SKILL.md` for invocation.

A skill listed but not invoked on its trigger is usually a pointer problem: check the description first, and the descriptions it competes with, before touching the body.

## Repo standard

Own skills live under `skills/<name>/` (public) or `private/<name>/`. A finished skill has:

1. `SKILL.md` with `name`, `description` and, for adoptions, `metadata.upstream` and `metadata.adopted`; optional `references/`, `scripts/`, `assets/`.
2. `agents/openai.yaml` with an `interface` block, plus `policy` for user-only.
3. A `manifest.yaml` entry: `kind: own`, `adopted: true` and `upstream: <owner/repo> <path>/SKILL.md@<sha>` for adoptions, `harnesses:` only when narrowed. `bin/upstream` parses that exact form.
4. `bin/link` run, `bin/link check` clean apart from known findings, `claude plugin validate` clean.
5. The headless test above passed in both harnesses.
6. No overlap with the skills sharing its triggers and no contradiction of `global/AGENTS.md` (`SKILL.md`, "A skill is done when").

The `manage-skills` skill runs the adoption interview and the manifest edits; this skill sets the writing and the standard.
