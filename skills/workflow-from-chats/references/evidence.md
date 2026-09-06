# Evidence

What a human turn is, what counts once, and how a cluster is graded. Record-level field checks per harness are in `transcript-sources.md`.

## A human turn

Evidence is text the user typed or dictated. Nearby assistant and tool messages serve only to understand what the user corrected and what happened next. Excluded from evidence: injected instructions (`AGENTS.md`, skill bodies, environment context), compaction summaries, system and meta records, tool results, task notifications, prompts written by another agent (headless runs, subagent dispatches, automatic reviews), and anything whose authorship the record cannot establish. Uncertain authorship stays uncertain and is reported as such, never counted.

A successful agent action, the user's silence, or a subagent's agreement establishes no preference. An explicit instruction for one task is strong evidence for that task and weak evidence for a global rule.

## Counting

One statement counts once. Forks, resumed sessions and subagent threads inherit history: deduplicate by message id, then by lineage (`forked_from_id`, `parent_thread_id`, `parentUuid`, subagent directories), then by timestamp plus normalised text within a known lineage. Independent conversations that say the same thing in the same words are separate evidence; a copy inside one lineage is not.

Within one scope the latest explicit revision wins. Different scopes (one project, one harness, one file type) keep separate rules; a later instruction in another scope does not retract an earlier one.

## Grades

- **Strong**: an explicit reusable preference or correction, or the same behaviour asked for in at least two independent conversations.
- **Medium**: plausible reusable guidance supported by user feedback, with recurrence or scope still uncertain; the report names the inference and what evidence is missing.
- **Weak**: an isolated or task-specific instruction. Listed as dismissed with the reason, not proposed.
- **Contradicted**: incompatible evidence that scope or a later revision does not explain. Shown as a decision for the user, with both sides quoted.

The number of proposals is whatever the evidence supports; an empty window yields a report that says so with the coverage figures.

## Evidence link

Every supporting or contrary turn is cited as: harness, the transcript as a clickable link (`<a href="file:///home/.../session.jsonl">` with the absolute path as the link text), locator (line number, plus message uuid for Claude Code or message id and `ordinal` for Codex), local timestamp, and an excerpt short enough to show the correction. Excerpts are escaped text, never inserted into markup, scripts or attributes.
