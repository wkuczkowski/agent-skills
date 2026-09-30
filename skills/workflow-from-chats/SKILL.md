---
name: workflow-from-chats
description: Mines the user's Codex and Claude Code conversations since the last run for recurring corrections and workflows, deduplicates them against manifest.yaml, and replies in the chat with short proposals (new skills, skill edits, instruction changes) the user can answer point by point. Use from the weekly skill review or when asked what recent chats suggest.
disable-model-invocation: true
metadata:
  upstream: cursor/plugins cursor-team-kit/skills/workflow-from-chats/SKILL.md
  upstream-commit: "bca612957941f8bad424d1856e7f46226a122d60"
  adopted: "2026-09-05"
---

# Workflow from chats

Turns what the user corrected, repeated or asked for in Codex and Claude Code conversations into proposals: a new skill, an edit to an existing skill, or a change to `global/AGENTS.md`. The repo is `/home/wkuczkowski/projects/TOOLS/skills`; run every command from it. The user's instructions take precedence over this skill. The skill proposes; nothing is installed or edited outside `reports/` until the user answers.

The user reads the chat reply and practically never a long report, so the reply is the deliverable. The evidence and ready diffs go to `reports/<date>/conversations.md` (`<date>` from `date +%F`), written for the agent that applies the accepted proposals and for the weekly review, not for him.

## Window

- Run `date -Is` first and keep the value: it is the window's end and, later, the new last-run timestamp. Start: `--since <YYYY-MM-DD or ISO timestamp>` in the prompt when given; otherwise the timestamp in `reports/.workflow-from-chats-last-run`; otherwise seven days before the end. Local time zone.
- Filter by message timestamps, since a session created earlier can hold turns inside the window.
- After the reply is ready, write that kept start-of-run value (one line) to `reports/.workflow-from-chats-last-run`, so the next run starts where this one's reading stopped. A run with `--since` records it too.

## Steps

1. **Gather.** Read [references/transcript-sources.md](references/transcript-sources.md) for where both harnesses keep transcripts and which fields identify a human turn, then [references/evidence.md](references/evidence.md) for what counts as evidence. Inventory the conversations in the window from both sources and extract the human-authored turns with their transcript path, locator and timestamp. Done when you can say what was examined, what was excluded and why, and which sources were unavailable.
2. **Cluster and grade.** Group the turns by recurring workflow, correction or preference, across conversations rather than within one. Grade each cluster with the scale in `evidence.md`. Done when every cluster carries its supporting turns, contrary turns and a grade.
3. **Deduplicate against what exists.** `manifest.yaml` lists every skill the user has; the descriptions come from `grep -h '^description:' skills/*/SKILL.md private/*/SKILL.md .agents/skills/*/SKILL.md`; `global/AGENTS.md` holds the global instructions; `system/` holds the harness built-ins. A cluster that matches an existing skill becomes an edit proposal against that skill, or is dismissed with the matching skill named; only a cluster nothing covers becomes a new-skill proposal. Done when every cluster names the skill or file it was compared with.
4. **Write the working file** `reports/<date>/conversations.md` in the form in [references/proposals.md](references/proposals.md): per proposal the exact wording or diff and its evidence; dismissed clusters with the reason; the coverage figures. Done when an agent could apply any proposal from that file alone.
5. **Reply and record.** Reply in the chat in the user's language, in the form in `proposals.md`. Write the last-run file.

## Data

Transcripts may contain the user's and employees' data; the working file and the reply quote only the short excerpts a proposal needs and leave out credentials and personal identifiers. Material that looks like law-firm client data is set aside and named in the coverage line. Transcripts are read, never edited; scratch copies go to a temporary directory outside the repo.
