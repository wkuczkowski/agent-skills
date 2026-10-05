---
name: workflow-from-chats
description: Mines the user's Codex and Claude Code conversations since the last run for recurring corrections and workflows, deduplicates them against manifest.yaml, and replies in the chat with proposals (new skills, skill edits, instruction changes) the user can decide on easily. Use from the weekly skill review or when asked what recent chats suggest.
disable-model-invocation: true
metadata:
  upstream: cursor/plugins cursor-team-kit/skills/workflow-from-chats/SKILL.md
  upstream-commit: "bca612957941f8bad424d1856e7f46226a122d60"
  adopted: "2026-09-05"
---

# Workflow from chats

Finds what the user corrected, repeated or asked for in his Codex and Claude Code conversations since the last run, and turns what recurs into proposals: a new skill, an edit to a skill, or a change to `global/AGENTS.md`. Each proposal is checked against what already exists (`manifest.yaml`, the skills' descriptions, `global/AGENTS.md`, the harness built-ins under `system/`), so a match becomes an edit or is dropped. The repo is `/home/wkuczkowski/projects/TOOLS/skills`. The user's instructions take precedence over this skill.

The run is done when he has a reply in the chat, in his language, that lets him decide on each proposal easily, and the agent that later applies his answers has what it needs (evidence and the exact wording or diff per proposal) in `reports/<date>/conversations.md`. He practically never reads a long report; a 111 KiB HTML report on 2026-09-30 went unread. Nothing outside `reports/` changes until he answers.

## Window

From the timestamp in `reports/.workflow-from-chats-last-run`, or `--since` when the prompt gives one, or seven days back when neither exists, to the start of this run; the start of this run is written to that file once the reply is ready. Sessions created earlier can hold turns inside the window.

## What has mattered in past runs

- Where the transcripts are and which records are the user's own words: [references/transcript-sources.md](references/transcript-sources.md). Prompts from headless runs, subagents and automatic reviews look like user turns in both harnesses, and forked or resumed sessions repeat earlier history, which once inflated counts.
- One instruction for one task is weak evidence for a global rule; the same correction in independent conversations is strong.
- A proposal that records a lesson or something he rejected ages better than one that records what he likes, which can stop an agent from proposing something better.
- A new-skill proposal carries what `manage-skills` needs to write it; the mechanics are in the `writing-for-agents` skill.

## Data

Transcripts hold the user's and employees' data. Proposals and the working file quote only the short excerpts a proposal needs, without credentials or personal identifiers. Material that looks like law-firm client data is set aside and named as excluded. Transcripts are read, never edited; scratch copies go to a temporary directory outside the repo.
