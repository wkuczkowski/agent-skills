# Problem reporting

For a Cursor CLI or harness problem (authentication, unavailable or misresolved models, transport errors, workspace trust, startup state, permission plumbing, malformed events, crashes, stalls), not for a bug in the task or an ordinary model mistake.

1. Keep the failed run's stdout and stderr.
2. Write a Markdown report `YYYYMMDD-HHMMSS-<short-slug>.md` under `~/.local/state/headless-harness-reports/cursor/`, or `/tmp/headless-harness-reports/cursor/` if the parent sandbox cannot write there. Do not ask for broader access only to store it.
3. Include the timestamp, CLI version, working directory, sanitized command, exit code, failure phase, a short stderr excerpt, the `system/init` event, the last useful event, and the recovery attempted.
4. Leave out prompts, source data, environment dumps, tokens, credentials, account identifiers and unrelated configuration.
5. Retry once when the failure looks transient, or use a route that keeps the selected model and Auto Review; otherwise leave only the affected part undone.
