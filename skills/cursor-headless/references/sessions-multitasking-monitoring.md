# Sessions, multitasking, and monitoring

Read this file when a Cursor task needs follow-up turns, parallel subagents, or background monitoring.

## Sessions

```bash
sid=$(cursor-agent -p --trust --auto-review --sandbox disabled \
  --model cursor-grok-4.6-high-fast "Analyze X" \
  --output-format json | jq -r .session_id)

cursor-agent -p --trust --auto-review --sandbox disabled \
  --model cursor-grok-4.6-high-fast --resume "$sid" \
  "Follow-up: ..." --output-format stream-json
```

`--continue` resumes the most recent session. `cursor-agent ls` lists sessions.

## Multitasking

Prefix the prompt with `/multitask` and give each requested subagent a self-contained task with absolute paths. Cursor may skip subagents for work it considers trivial.

```bash
cursor-agent -p --trust --auto-review --sandbox disabled \
  --model cursor-grok-4.6-high-fast --output-format stream-json \
  "/multitask Use parallel subagents: subagent 1 <task A>; subagent 2 <task B>. Combine both reports." \
  >events.jsonl 2>err.log &
```

- A started `taskToolCall` records the requested subagent description and prompt.
- Its completed event confirms launch and contains `.result.success.agentId`. It does not mean the subagent finished.
- The parent narrates subagent completion. A subagent chat can be inspected with `--resume <agentId>`.

Extract launched subagent ids with:

```bash
jq -r 'select(.type=="tool_call" and .subtype=="completed")
       | .tool_call.taskToolCall | select(.)
       | .args.description + " " + (.result | fromjson | .success.agentId)' events.jsonl
```

## Monitoring

- A `tool_call` event with `subtype: started` and no matching completion is in flight.
- If the stream stops growing for several minutes, inspect the last started tool and the process state.
- Completion requires a final `result` event with `subtype: success` and `is_error: false`.
- `--stream-partial-output` adds text deltas when the caller needs them.

Inspect recent tool calls with:

```bash
jq -r 'select(.type=="tool_call" and .subtype=="started") | .tool_call
       | to_entries[0] | "\(.key) \(.value.args.command // .value.args.path // "")"' \
  events.jsonl | tail -3
```
