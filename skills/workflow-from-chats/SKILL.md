---
name: workflow-from-chats
description: Mines the user's Codex and Claude Code conversations since the last run for recurring corrections and workflows, deduplicates them against manifest.yaml, and writes an HTML report of proposals (new skills, skill edits, instruction diffs) in the form manage-skills new takes, linked to the transcript turns behind each. Use from the weekly skill review or when asked what recent chats suggest.
disable-model-invocation: true
metadata:
  upstream: cursor/plugins cursor-team-kit/skills/workflow-from-chats/SKILL.md
  upstream-commit: "bca612957941f8bad424d1856e7f46226a122d60"
  adopted: "2026-09-05"
---

# Workflow from chats

Turns what the user corrected, repeated or asked for in Codex and Claude Code conversations into proposals: a new skill, an edit to an existing skill, or a change to `global/AGENTS.md`. The repo is `/home/wkuczkowski/projects/skills`; run every command from it. The report is `reports/<date>/conversations.html` (`<date>` from `date +%F`) on the house template, `assets/report/README.md`. The user's instructions take precedence over this skill. The skill proposes; nothing is installed or edited outside `reports/`.

## Window

- Run `date -Is` first and keep the value: it is the window's end and, later, the new last-run timestamp. Start: `--since <YYYY-MM-DD or ISO timestamp>` in the prompt when given; otherwise the timestamp in `reports/.workflow-from-chats-last-run`; otherwise seven days before the end. Local time zone.
- Filter by message timestamps, since a session created earlier can hold turns inside the window.
- After the report is written, write that kept start-of-run value (one line) to `reports/.workflow-from-chats-last-run`, so the next run starts where this one's reading stopped. A run with `--since` records it too.

## Steps

1. **Gather.** Read [references/transcript-sources.md](references/transcript-sources.md) for where both harnesses keep transcripts and which fields identify a human turn, then [references/evidence.md](references/evidence.md) for what counts as evidence. Inventory the conversations in the window from both sources; extract the human-authored turns with their transcript path, locator and timestamp; each becomes an evidence entry with a `file://` link to the transcript. Done when the inventory can state what was examined, what was excluded and why, and which sources were unavailable.
2. **Cluster and grade.** Group the turns by recurring workflow, correction or preference, across conversations rather than within one. Grade each cluster with the scale in `evidence.md`. Done when every cluster carries its supporting turns, contrary turns and a grade.
3. **Deduplicate against what exists.** `manifest.yaml` lists every skill the user has; the descriptions come from `grep -h '^description:' skills/*/SKILL.md private/*/SKILL.md .agents/skills/*/SKILL.md`; `global/AGENTS.md` holds the global instructions; `system/` holds the harness built-ins. A cluster that matches an existing skill by name or by description overlap becomes an edit proposal against that skill, or is dismissed with the matching skill named; only a cluster nothing covers becomes a new-skill proposal. Done when every cluster names the skill or file it was compared with.
4. **Write the proposals** in the forms in [references/proposals.md](references/proposals.md): new skill, skill edit, instruction change. Strong and medium clusters become proposals; weak ones are listed as dismissed with the reason; contradicted ones are shown as a decision for the user. Done when every proposal has its exact wording or diff, its evidence links and a check the user can run.
5. **Build the report** from the section list in `proposals.md` with `assets/report/build`. Done when the build exits 0 and the file is at `reports/<date>/conversations.html`.
6. **Record and reply.** Write the last-run file. Reply with the report path, the count of proposals per grade, and the coverage limits in one short paragraph.

## Data

Transcripts may contain the user's and employees' data; the report quotes only the short excerpts a proposal needs and leaves out credentials and personal identifiers. Material that looks like law-firm client data is set aside and named in the coverage section. Transcripts are read, never edited; scratch copies go to a temporary directory outside the repo.
