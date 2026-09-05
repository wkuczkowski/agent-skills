# Sessions and monitoring

Read this file when a Codex task needs follow-up turns or will run long enough to monitor.

Choose `low` or `high` using the model and effort guidance in `SKILL.md`. The examples below use `high`.

## Resume

```bash
codex exec resume -m gpt-6-astra -c model_reasoning_effort=high \
  -c approval_policy=on-request -c approvals_reviewer=auto_review \
  --last "follow-up question"

codex exec resume -m gpt-6-astra -c model_reasoning_effort=high \
  -c approval_policy=on-request -c approvals_reviewer=auto_review \
  <SESSION_ID> "follow-up"
```

Do not use `--ephemeral` for a session that must be resumed. Get its id from the `thread.started` event.

## Background runs

```bash
codex exec --json -m gpt-6-astra -c model_reasoning_effort=high --sandbox workspace-write \
  -c approval_policy=on-request -c approvals_reviewer=auto_review \
  "<long task>" >events.jsonl 2>progress.log &
```

- An `item.started` event without a matching `item.completed` event is the current action.
- If `events.jsonl` stops growing for several minutes, inspect the last started item and the process state.
- Completion requires a terminal `turn.completed` event. The last completed `agent_message` is the final answer.
- Resume with the `thread_id` from `thread.started`.

Inspect recent items with:

```bash
jq -r 'select(.type|startswith("item"))
       | "\(.type) \(.item.type) \(.item.command // .item.text // "" | tostring | .[0:80])"' \
  events.jsonl | tail -3
```
