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

Do not use `--ephemeral` for a session that must be resumed. Get its id from the `thread.started` event: it is the first stdout line and is present even in a run killed seconds after launch.

`resume` accepts neither `--sandbox` nor `-C`, and it does not inherit them from the original run: a session started with `--sandbox read-only` resumed as the `config.toml` default, `workspace-write`. On every resume pass `-c sandbox_mode=<mode>` and run the command from the original working directory. Check the result in the rollout's `turn_context` records (SKILL.md, Output and completion).

`--last` takes the most recent session on the machine; under parallel runs that is someone else's, so resume by id.

## Background runs

```bash
nohup timeout -k 15 3600 codex exec --json -m gpt-6-astra -c model_reasoning_effort=high \
  --sandbox workspace-write -c approval_policy=on-request -c approvals_reviewer=auto_review \
  < prompt.txt >events.jsonl 2>progress.log &
```

- An `item.started` event without a matching `item.completed` event is the current action.
- Events carry no timestamps and are sparse: a command appears when it starts and when it completes, nothing in between. Seconds since the stream last grew: `echo $(( $(date +%s) - $(stat -c %Y events.jsonl) ))`. Past 600 with no long command in flight, inspect the last started item and the process state.
- Subagent activity shows only as `collab_tool_call` items with `tool: "wait"`; spawns and subagent output are not itemised.
- Completion requires a terminal `turn.completed` event. The last completed `agent_message` is the final answer.
- Resume with the `thread_id` from `thread.started`.

Inspect recent items with:

```bash
jq -r 'select(.type|startswith("item"))
       | "\(.type) \(.item.type) \(.item.command // .item.text // "" | tostring | .[0:80])"' \
  events.jsonl | tail -3
```
