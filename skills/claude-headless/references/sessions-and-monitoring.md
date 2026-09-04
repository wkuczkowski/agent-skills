# Sessions and monitoring

Read this file when a Claude task needs follow-up turns or will run long enough to monitor.

## Resume

```bash
sid=$(claude -p "Analyze X" \
  --model fable --effort high \
  --permission-mode auto --permission-prompts none \
  --strict-mcp-config --output-format json | jq -r .session_id)

claude -p "Follow-up: ..." --resume "$sid" \
  --model fable --effort high \
  --permission-mode auto --permission-prompts none \
  --strict-mcp-config --output-format json | jq -r .result
```

Do not use `--no-session-persistence` for resumable work. `--continue` resumes the most recent session for the current directory.

## Monitoring

```bash
claude -p "<long task>" \
  --model fable --effort high \
  --permission-mode auto --permission-prompts none \
  --strict-mcp-config --output-format stream-json --verbose \
  >events.jsonl 2>err.log &
```

- Add `--include-partial-messages` for token deltas.
- Add `--forward-subagent-text` when the caller needs subagent text.
- The last JSONL record is the terminal `result` event.
- Track the latest assistant `tool_use` event and the file's modification time to detect a stalled run.
- `--max-turns N` limits tool-use turns. `--max-budget-usd X` limits the client-side cost estimate.
- Claude has no general wall-clock timeout flag for print mode. The parent process must enforce one. SIGTERM exits with code 143, and a persisted session can be resumed.

Native `claude --bg` sessions are separate from print mode. Manage them with `claude agents`, `claude logs`, `claude attach`, `claude stop`, and `claude rm`.
