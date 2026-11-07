# Problem reporting

For a Codex CLI or harness problem (authentication, unavailable model, transport errors, startup state, sandbox initialization, permission plumbing, malformed JSONL, crashes, stalls), not for a bug in the user's task or an ordinary model mistake.

- The report goes to `~/.local/state/headless-harness-reports/codex/YYYYMMDD-HHMMSS-<short-slug>.md`, or `/tmp/headless-harness-reports/codex/` when the parent sandbox cannot write there; broader access is not requested only to store it.
- It holds the timestamp, CLI version, working directory, sanitized command and flags, exit code, failure phase, a short stderr excerpt, the last useful JSONL event, and the recovery attempted. It leaves out prompts, source data, environment dumps, tokens, credentials, account identifiers and unrelated configuration.
- The user hears what failed, its impact and the report path; unaffected work continues. A transient-looking failure gets one retry, or an alternative that keeps the selected model, Auto-review and sandbox; with no safe route, only the affected part stays undone.
