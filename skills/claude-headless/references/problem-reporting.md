# Problems and recovery

For failures of the CLI or harness, not defects in the delegated code.

- Exit 143 is SIGTERM; the turn in progress records no result. Whether the parent sent it or something else did stays unknown until evidenced: an exit near a Bash-tool timeout is a correlation, not proof of which timer sent the signal. Inspect saved edits before resuming.
- An exhausted account quota with a reset time: report the reset time with its timezone and stop retrying that quota; continue independent work or a model alternative the user has already authorized. How a quota failure looks in the result is in [observed in use](observed-in-use.md).
- A revoked or expired login yields an error starting with `Failed to authenticate` (2.1.287+). `claude auth status` shows the state.
- A missing terminal result or a quiet log is not proof the process ended; see [sessions and monitoring](sessions-and-monitoring.md).

## Save evidence

Keep raw run outputs private. Write a sanitized report under `~/.local/state/headless-harness-reports/claude/` (the codex and cursor skills use sibling directories), or `/tmp/headless-harness-reports/claude/` when the parent cannot write there; do not request extra access just for the report. Name it `YYYYMMDD-HHMMSS-<slug>.md`.

Include the CLI version, working directory, launch, last-event and end times, sanitized flags, the actual exit status or "unknown", result-field summaries, denied actions and recovery tried. Separate confirmed facts, hypotheses and missing evidence. Credentials, account identifiers, full prompts, private source, transcripts and environment dumps stay out of the report. Tell the user the impact and where the report is.
