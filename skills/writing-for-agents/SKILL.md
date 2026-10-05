---
name: writing-for-agents
description: Holds what earns a line in a skill or instruction file in this collection, and what makes a skill here finished (frontmatter, invocation flags, how it is proven to fire). Use when creating or editing a skill or its description, AGENTS.md or CLAUDE.md, when deciding whether a skill is model-invocable or user-only, or when a skill fails to fire on its trigger.
metadata:
  upstream: mattpocock/skills skills/productivity/writing-for-agents
  upstream-commit: "321658273cb1d20b76026717d027d505790106d4"
  adopted: "2026-09-06"
---

# Writing for agents

## What earns a line

The models these files are written for are capable and change every few weeks. A line earns its place when removing it would change what an agent decides:

- **Facts the agent cannot find on its own**: paths, commands, URLs, how this machine and repo are set up. Dated, with what they were checked against.
- **Lessons from what went wrong**, dated. A lesson ages slowly and does not narrow the agent's options.
- **What the user dislikes or has rejected.** What he likes is riskier to write down: an agent may stop there instead of proposing something better.
- **The goal and what done looks like**, not the steps; the method is the agent's. Steps stay only where a wrong order breaks something.

How the agent talks to the user (questions, reports) is left to the model and the harness. Imperatives sit only where a wrong guess costs data, security, an irreversible action or a decision the user made. A skill that could conflict with the user's request says his instructions take precedence; without that line, GPT-6 Astra stalled on such skills (2026-09).

## Facts the harnesses fix

- The description decides whether a skill fires. On 2026-10-05 `domain-modeling` did not fire in Claude Code until its description named the situation the user was in ("explains what he means by a word"). Every description sits in context on every turn, and the Codex catalog caps all of them at about 8,000 characters (`bin/link check` reports the total).
- `name` equals the directory (lowercase letters, digits, hyphens). `claude plugin validate` does not check it.
- Each harness reads only its own invocation flag (loading test, 2026-09-06). User-only needs `disable-model-invocation: true` in the frontmatter (Claude Code) and `policy.allow_implicit_invocation: false` in `agents/openai.yaml` (Codex); `bin/link check` reports a mismatch. `agents/openai.yaml` also carries `interface` (`display_name`, `short_description` of 25 to 64 characters, `default_prompt` naming `$<name>`).
- Codex silently skips a skill whose `SKILL.md` is a symlink (2026-09-06); the directory and `references/` may be symlinks. A Codex session started inside this repo sees every vendor skill under `.agents/skills`, whatever the manifest says, so skill tests run from outside it.
- Claude Code loads a directory's `AGENTS.md` when it has no `CLAUDE.md` (2.1.285), so one `AGENTS.md` serves both harnesses, as the user settled on 2026-09-22.

## A skill is done when

For a new skill or a material change; a small edit needs 1 and 2. The user may waive any item.

1. `claude plugin validate --strict skills` passes. It accepts only a directory named `skills`, so a private skill is validated from a scratch copy.
2. The `manifest.yaml` entry exists, `bin/link` has run and `bin/link check` is clean.
3. A headless run per harness (`claude-headless`, `codex-headless`) with a matching prompt shows the skill invoked in the harness record: a Skill tool call in the Claude Code transcript, a read of `SKILL.md` in the Codex rollout. The model's own report does not count; Codex misreported three runs out of six in a test on 2026-09-06.
4. Nothing it names (path, flag, command) is missing, and no other skill or the global instructions say otherwise.
