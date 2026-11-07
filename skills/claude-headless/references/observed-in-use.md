# Observed in use

What happened in real and test `claude -p` runs on this machine. Each note has its date and the CLI version. The CLI changes often, so a note is history, not a rule: if the current CLI behaves differently, the current behaviour wins, and the note should be updated or removed. Raw records: `research/claude-codex-headless-empirical-2026-09-20.md`, `research/claude-headless-recheck-2026-11-07.md` in the agent-skills repo.

## Background subagents and the print-mode ceiling

- 2026-11-07, 2.1.316, Haiku at low effort, `CLAUDE_CODE_PRINT_BG_WAIT_CEILING_MS=20000`. The main agent launched one background subagent running a 50 s foreground command and ended its turn. After 20 s the run printed `Background tasks still running 20s after the last turn (subagent "<description>"); stopping them. ...` on stderr and exited 0 with `subtype: success`, `terminal_reason: completed`, `is_error: false`; only `subagent_stats` showed it (`spawned: 1`, `completed: 0`, `killed.system: 1`). The headless docs fetched the same day say background subagents keep the run open until they complete; the run did not. With the ceiling at `0` this does not apply.
- 2026-11-07, 2.1.316, same setup with a subagent told to `sleep 75`: the subagent's long foreground sleep was blocked, so it moved the command to the background and added a Monitor watch. The run took two more turns, then stopped the shell and the watch at the ceiling. `subagent_stats.killed` stayed all zero and `completed: 0`; the stderr line named the shell and the monitor.
- 2026-09-18, 2.1.274: a 28-minute orchestration lost five subagents to the 600 s default ceiling; the result record was clean.

## Kills and resume

- 2026-09-20, 2.1.278: four parallel subagents, process killed at 100 s. The stream already held a `result` with `subtype: success` whose text said the agents were still running. On `--resume`, the agent re-reached all four with SendMessage from their saved transcripts; three of four delivered within the next 400 s.
- 2026-09-20, 2.1.278: `timeout 60` (SIGTERM) gave exit 124 and no `result`. A run killed 4 to 6 s after launch still persisted the prompt, and resume knew the task; the in-flight Read had completed.

## Time

- 2026-09-20, 2.1.278, Opus at low effort: a web lookup took under 1 minute, bounded repository research 2 to 5 minutes, a task with subagents 10 minutes and more. Longest gap between stream events: 63 s solo, 91 s with subagents.
- 2026-09-20, 2.1.278: this prompt wording made the agent finish early with a normal result and a "Not covered" list (a 4-minute budget ended at 128 s, so state the budget you can really afford):

  > Time budget: 10 minutes of wall-clock time from your first action. Run `date +%s` first and again after every few tool calls. When 8 minutes have passed, stop exploring and deliver the report with what you have, listing the areas you did not reach under a heading "Not covered".

- 2026-09-20: for work expected to pass ten minutes, a progress file (findings appended to an absolute path after each area, the final report in a second file) left usable output when a run was cut.

## Other

- 2026-09 (version not recorded): an exhausted quota produced `subtype: success` together with `is_error: true`, `terminal_reason: api_error` and exit 1.
- 2026-09-20, 2.1.278: web search worked in `-p` with no extra configuration (2 searches, 0 `permission_denials`).
- 2026-11-07, 2.1.316: the one-shot command in SKILL.md, Haiku at low effort, "Reply with the word ok", run from a directory outside any repo: exit 0 in 14 s, `subtype: success`, `terminal_reason: completed`, `is_error: false`, `permission_denials: []`, `modelUsage` keys `claude-haiku-5-5` only, empty stderr. The result also carries `stop_reason`, `api_error_status`, `safety_stops`, `subagent_stats`, `result_index`, `process_turn_index`, `queued_turn_count`.
