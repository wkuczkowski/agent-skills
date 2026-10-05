---
name: writing-for-agents
description: Holds what earns a line in a skill or an AGENTS.md written for agents, and the harness facts that decide whether a skill loads and fires in Claude Code and Codex. Use when creating or editing a skill or its description or an AGENTS.md in any project, when deciding whether a skill is model-invocable or user-only, or when a skill fails to fire on its trigger.
metadata:
  upstream: mattpocock/skills skills/productivity/writing-for-agents
  upstream-commit: "321658273cb1d20b76026717d027d505790106d4"
  adopted: "2026-09-06"
---

# Writing for agents

## What earns a line

The models these files are written for are capable and change every few weeks. A line earns its place when removing it would change what an agent decides:

- **Facts the agent cannot find on its own**: paths, commands, URLs, how the machine and the project are set up. Dated, with what they were checked against.
- **Lessons from what went wrong**, dated. A lesson ages slowly and does not narrow the agent's options.
- **What the user dislikes or has rejected.** What he likes is riskier to write down: an agent may stop there instead of proposing something better.
- **The goal and what done looks like**, not the steps; the method is the agent's. Steps stay only where a wrong order breaks something.

How the agent talks to the user (questions, reports) is left to the model and the harness. Imperatives sit only where a wrong guess costs data, security, an irreversible action or a decision the user made. A skill that could conflict with the user's request says his instructions take precedence; without that line, GPT-6 Astra stalled on such skills (2026-09).

## Instruction files

The user keeps one `AGENTS.md` per project and no `CLAUDE.md` (settled 2026-09-22, restated 2026-10-05): Claude Code loads `AGENTS.md` where a directory has no `CLAUDE.md` (2.1.285), and Codex reads `AGENTS.md` from the repo root down to the working directory, 32 KiB combined.

## Skills: what the harnesses decide

- The description decides whether a skill fires. On 2026-10-05 `domain-modeling` did not fire in Claude Code until its description named the situation the user was in ("explains what he means by a word"). Every description sits in context on every turn, and the Codex catalog caps all of them together at about 8,000 characters.
- `name` equals the directory (lowercase letters, digits, hyphens). `claude plugin validate` does not check it.
- Each harness reads only its own invocation flag (loading test, 2026-09-06). User-only needs `disable-model-invocation: true` in the frontmatter (Claude Code) and `policy.allow_implicit_invocation: false` in `agents/openai.yaml` (Codex). `agents/openai.yaml` also carries `interface` (`display_name`, `short_description` of 25 to 64 characters, `default_prompt` naming `$<name>`).
- Codex silently skips a skill whose `SKILL.md` is a symlink (2026-09-06); the directory and `references/` may be symlinks.
- Whether a skill fires is shown by the harness record, not by the model's own report: a Skill tool call in the Claude Code transcript, a read of `SKILL.md` in the Codex rollout. Codex misreported three runs out of six in a test on 2026-09-06. The `claude-headless` and `codex-headless` skills run such a test.

The user's instructions take precedence over this skill.
