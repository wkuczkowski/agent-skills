# Sessions, multitasking, and monitoring

Read this file when a Cursor task needs follow-up turns, parallel subagents, background monitoring, or many independent runs.

## Sessions

```bash
sid=$(cursor-agent -p --trust --auto-review --sandbox disabled \
  --model cursor-grok-4.6-high "Analyze X" \
  --output-format json | jq -r .session_id)

cursor-agent -p --trust --auto-review --sandbox disabled \
  --model cursor-grok-4.6-high --resume "$sid" \
  "Follow-up: ..." --output-format stream-json
```

`--continue` resumes the most recent session. `cursor-agent ls` lists sessions.

## Multitasking

Inside one session, delegate with the native Task subagent. Prefix the prompt with `/multitask` and give each requested subagent a self-contained task with absolute paths. Cursor may skip subagents for work it considers trivial. That stays in-process; it does not start another `cursor-agent`.

```bash
cursor-agent -p --trust --auto-review --sandbox disabled \
  --model cursor-grok-4.6-high --output-format stream-json \
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

Launch mass parallel runs from an external runner: a bash or Python script that starts N independent `cursor-agent -p` processes. Auto Review blocks a nested `cursor-agent` launched from inside a `--auto-review` session: the child never starts, and the parent does the work itself.

```bash
nohup cursor-agent -p --trust --auto-review --sandbox disabled \
  --workspace "$PWD" --model cursor-grok-4.6-high \
  "$prompt" --output-format stream-json \
  < /dev/null > "$run_dir/$name.jsonl" 2> "$run_dir/$name.err" &
```

Read completion from the last `result` event in that jsonl. If the task asked for a result file and the file is missing, extract JSON from that event as in SKILL.md.

## Monitoring

- A `tool_call` event with `subtype: started` and no matching completion is in flight.
- If the stream stops growing for several minutes, inspect the last started tool and the process state.
- Completion requires a final `result` event with `subtype: success` and `is_error: false`.
- `--stream-partial-output` adds text deltas when the caller needs them.
- Count live runs with `pgrep -fc "cursor-agent.*--model"`. `pgrep -c cursor-agent` returns 0 because the process name is `node`.

Inspect recent tool calls with:

```bash
jq -r 'select(.type=="tool_call" and .subtype=="started") | .tool_call
       | to_entries[0] | "\(.key) \(.value.args.command // .value.args.path // "")"' \
  events.jsonl | tail -3
```
