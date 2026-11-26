---
name: writing-for-agents
description: Holds what earns a line in a skill or an AGENTS.md written for agents, and the harness facts that decide whether a skill loads and fires in Claude Code, Codex and Antigravity. Use when creating or editing a skill or its description or an AGENTS.md in any project, when deciding whether a skill is model-invocable or user-only, or when a skill fails to fire on its trigger.
metadata:
  upstream: mattpocock/skills skills/productivity/writing-for-agents
  upstream-commit: "321658273cb1d20b76026717d027d505790106d4"
  adopted: "2026-09-06"
---

# Writing for agents

## What earns a line

The models these files are written for are capable and change every few weeks. A line earns its place when removing it would change what an agent decides:

- **Facts the agent cannot find on its own**: paths, commands, URLs, how the machine and the project are set up. Dated, with what they were checked against. Anthropic's test (Claude Code best practices, 2026-11): could the agent find it by reading the code, and would it actually go and look? If both, leave it out.
- **Lessons from what went wrong**, dated. A lesson ages slowly and does not narrow the agent's options.
- **What the user dislikes or has rejected.** What he likes is riskier to write down: an agent may stop there instead of proposing something better.
- **The goal and what done looks like**, not the steps; the method is the agent's. Steps stay only where a wrong order breaks something.

How the agent talks to the user (questions, reports) is left to the model and the harness except where global instructions or a dedicated skill (such as `reporting-to-the-user`) record what he rejected or explicitly asked for (2026-11-26). Imperatives sit only where a wrong guess costs data, security, an irreversible action or a decision the user made. A skill that could conflict with the user's request says his instructions take precedence; without that line, GPT-6 Astra stalled on such skills (2026-09). Since Codex 0.16x the base prompt itself ranks the user above skills and proceeds unless a skill explicitly requires approval, so an approval the user does want is stated outright, not implied (2026-11).

How hard a model thinks is set by its effort level, not by wording: "think hard" or "be thorough" in a skill does little on the Claude 5.5 models, GPT-6.2 Sol or Gemini 4. On GPT-6.2 (Sol and Astra), also avoid generic brevity instructions that can truncate needed detail; state explicit completion and verification expectations instead (vendor guides, 2026-11; `research/skills-and-instructions-2026-11-20.md`). A skill never asks for the model's reasoning, a scratchpad or a running log of its thinking in the output; on Claude that can trigger a `reasoning_extraction` refusal. Asking for evidence or a summary of what was done is fine (`research/claude-5.5-instructions-2026-11-07.md`).

## Instruction files

The user keeps one `AGENTS.md` per project and no `CLAUDE.md` (settled 2026-09-22, restated 2026-10-05):

- **Claude Code** loads `AGENTS.md` where neither the directory nor any parent has a `CLAUDE.md` or `CLAUDE.local.md` (since 2.1.277; `~/.claude/CLAUDE.md` does not count, a project `CLAUDE.local.md` turns it off), and also loads nested `AGENTS.md` files when reading files inside subdirectories.
- **Codex** reads `AGENTS.md` from the repo root down to the starting directory, 32 KiB combined.
- **Antigravity** reads `AGENTS.md` / `GEMINI.md` from the workspace folder up to the repo root (24 KB per file, 20,000-token aggregate rules budget) and **does not load child `AGENTS.md` files** below the workspace root (checked 2026-11-20). Rules that must apply when starting from the repo root belong in the root `AGENTS.md` or a short path-conditioned pointer there.

The global file holds what sessions across the machine need; setup of one tool, connector or project goes to that project or `~/projects/troubleshooting` (user, 2026-11-02: "nie w globalnym AGENTS.md").

## Skills: what the harnesses decide

- The description decides whether a skill fires. On 2026-10-05 `domain-modeling` did not fire in Claude Code until its description named the situation the user was in ("explains what he means by a word"). Front-load the triggering situation in the description: model-invocable descriptions enter context on every turn up to each harness's budget (Agent Skills spec caps descriptions at 1,024 chars; Codex caps the whole catalog at 2% of context by default or ~8,000 chars fallback and shortens or omits entries past the budget).
- `name` equals the directory (lowercase letters, digits, hyphens). `claude plugin validate` does not check it.
- User-only needs `disable-model-invocation: true` in the frontmatter (read by Claude Code and Antigravity; verified on Antigravity ACP 2026-11-20) and `policy.allow_implicit_invocation: false` in `agents/openai.yaml` (Codex; loading test 2026-09-06). `agents/openai.yaml` also carries `interface` (`display_name`, `short_description` of 25 to 64 characters, `default_prompt` naming `$<name>`).
- A prompt that confines the agent to a folder kept it from opening a skill it had picked: told "pracuj tylko na plikach z tego folderu", GPT-6.2 Astra announced the skill and applied its description alone in 2 of 2 runs, then read `SKILL.md` in 2 of 2 once the sentence was gone (Codex 0.170.0, 2026-11-18). A brief that limits where an agent works says that skills stay open to it.
- Symlinks: Codex silently skips a skill whose `SKILL.md` is a symlink (2026-09-06), while directory symlinks work in Claude Code, Codex and Antigravity (`~/.gemini/config/skills/` verified 2026-11-20). T3 Code's Antigravity ACP symlinks `config/skills` from `~/.gemini/config/skills` automatically, but does not symlink `AGENTS.md` (`bin/link` maintains it).
- Whether a skill fires is shown by the harness record, not by the model's own report: a Skill tool call in Claude Code, a read of `SKILL.md` in Codex, or a `read_file` of `SKILL.md` in Antigravity. Test both routing (realistic Polish prompts and nearby negatives) and output quality against a no-skill or prior-version baseline—on 2026-11-22 a rewrite of `reporting-to-the-user` improved phrasing while silently dropping two findings that the no-skill baseline kept. Codex misreported three runs out of six in a test on 2026-09-06. The `claude-headless`, `codex-headless` and `antigravity-headless` skills run such a test.

The user's instructions take precedence over this skill.
