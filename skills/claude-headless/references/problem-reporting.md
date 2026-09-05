# Problems and recovery

Use this for CLI/harness failures, not ordinary defects in the delegated code.

| Observation | Next step |
|---|---|
| Missing terminal result or quiet log | Check the original execution handle and process visibility before declaring termination or restarting. |
| Exit 143 | Record interruption. Establish whether the parent intentionally sent SIGTERM; otherwise the sender/cause is unknown until evidenced. Inspect saved edits before a confirmed-safe resume. |
| Account quota exhausted with reset time | Preserve the checkpoint and report reset time with its timezone. Stop retrying that quota. Continue independent work or an already authorized model alternative. |
| Transient transport error | One bounded retry after confirmed termination, if it fits the task's deadline. Repeated errors need diagnosis, not a restart loop. |
| Auto/host approval denial | Identify the actual denied action. Use existing authorization and a permitted alternative; request approval only for a genuinely missing boundary. Keep Auto and inherited sandbox settings. |
| Exit 0 and clean result, but a required check failed or was not run | Treat implementation as unaccepted; return the specific finding or missing check. |

An observed `subtype: success` occurred with `is_error: true`, `terminal_reason: api_error`, exit 1 and exhausted quota. Checking subtype alone loses this failure. An unexpected exit near a Bash timeout is a correlation, not proof of which timer or process sent a signal.

## Save useful evidence

Preserve raw run outputs privately. Write a sanitized report under `~/.local/state/headless-harness-reports/claude/`, or `/tmp/headless-harness-reports/claude/` when the parent cannot write there. Do not request extra access just to store this report. Use `YYYYMMDD-HHMMSS-<slug>.md`.

Include version, working directory, launch/last-event/end times, sanitized flags, actual exit status or unknown, result-field summaries, denied actions, and recovery tried. Separate confirmed facts, hypotheses, and missing evidence. Keep credentials, account identifiers, full prompts, private source/transcripts, and environment dumps out of the report. Tell the user the impact and report location.

After an interrupted implementation, record what was saved, which version was reviewed, which tests actually ran, and what remains. An interrupted agent's partial work may be accepted through independent verification; neither an interruption nor a successful message alone decides artifact quality.
