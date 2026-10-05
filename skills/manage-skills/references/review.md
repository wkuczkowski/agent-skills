# Review

The review runs when the user asks for it, weekly or monthly. It tells him what in the collection needs a decision: skills nobody uses, upstream changes worth taking, drift between the manifest and the projections, what his recent conversations suggest, and contradictions between files an agent loads together. Monthly adds a refresh of the research notes and a check of every own skill and `global/AGENTS.md` against `writing-for-agents`.

## Facts

- Retirement candidates: six weeks without invocation for a model-invocable skill, twelve for a user-only one, `keep: true` exempt (`bin/usage --unused`). `bin/usage` counts reading a `SKILL.md` as use, so a skill whose only Codex use falls on days of work on this repo may be unused.
- `bin/upstream --json` exits 1 when a skill changed and `bin/link check` exits 1 when there are findings; both are results, not errors.
- Conversation candidates come from the `workflow-from-chats` skill. It is user-only, so the review follows it by reading its `SKILL.md`, not through the Skill tool.
- A contradiction worth reporting is two skills claiming the same trigger, a skill contradicting `global/AGENTS.md`, one rule stated twice in different words, or a path, flag or command that no longer exists.
- A vendor skill is not edited; a fix there is a manifest narrowing, an adoption or a line in an own file.
- The research refresh: [research-prompts.md](research-prompts.md).

## Output

Done when the user has a message in his language that lets him decide on each proposal easily, and the agent that applies his answers finds the evidence and a ready diff or command per proposal in `reports/<date>/`. Findings come from this run's command output, not from memory or an earlier review. He practically never reads long reports. The review proposes; the only files it writes are `usage.yaml`, the report directory, `reports/.workflow-from-chats-last-run` and, monthly, the new research files.
