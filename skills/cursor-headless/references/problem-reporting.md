# Problem reporting

Use this procedure for a Cursor CLI or harness problem, not for a bug in the user's task or an ordinary model mistake. Relevant problems include authentication, unavailable or misresolved models, transport errors, workspace trust, startup state, permission plumbing, malformed events, crashes, and stalls.

1. Preserve the failed run's stdout and stderr when available.
2. Write a Markdown report under `~/.local/state/headless-harness-reports/cursor/`. Use `/tmp/headless-harness-reports/cursor/` if the parent sandbox cannot write there. Do not request broader access only to store the report.
3. Name it `YYYYMMDD-HHMMSS-<short-slug>.md`.
4. Include the timestamp, CLI version, working directory, sanitized command and flags, exit code, failure phase, a short stderr excerpt, the `system/init` event, the final useful event, and the recovery attempted.
5. Exclude prompts, source data, environment dumps, tokens, credentials, account identifiers, and unrelated configuration.
6. Tell the user what failed, its impact, and the report path.
7. Continue unaffected work. Retry once when the failure looks transient, or use a documented alternative that preserves the selected model and Auto Review. Leave only the affected part incomplete when no safe route remains.

Cursor may need outer write access to `~/.cursor/projects`. Granting that access to the client process does not justify widening the task itself.
