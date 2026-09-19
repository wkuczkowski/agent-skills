# Sessions, multitasking, and monitoring

Read this file when a Cursor task needs follow-up turns, parallel subagents, background monitoring, many independent runs, or is driven from a Claude Code orchestrator.

## Sessions

```bash
sid=$(cursor-agent -p --trust --auto-review --sandbox disabled \
  --model cursor-grok-4.6-high --output-format json \
  < task.txt | jq -r .session_id)

cursor-agent -p --trust --auto-review --sandbox disabled \
  --model cursor-grok-4.6-high --resume "$sid" \
  --output-format stream-json < followup.txt
```

A resumed run keeps the same session id, and earlier tool results, MCP ones included, stay in context (measured 2026-09-19).

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
  --workspace "$PWD" --model cursor-grok-4.6-high --output-format stream-json \
  < "$run_dir/$name.prompt" > "$run_dir/$name.jsonl" 2> "$run_dir/$name.err" &
```

Read completion from the last `result` event in that jsonl. If the task asked for a result file and the file is missing, extract JSON from that event as in SKILL.md.

## Monitoring

- A `tool_call` event with `subtype: started` and no matching completion is in flight.
- If the stream stops growing for several minutes, inspect the last started tool and the process state.
- Completion requires a final `result` event with `subtype: success` and `is_error: false`.
- A `completed` tool_call carries `startedAtMs` and `completedAtMs`; their difference is the call's duration in ms.
- The stream is the full record. Cursor's own transcript under `~/.cursor/projects/<slug>/agent-transcripts/` keeps no tool results, so keep the jsonl.
- `--stream-partial-output` adds text deltas when the caller needs them.
- Count live runs with `pgrep -fc "cursor-agent.*--model"`. `pgrep -c cursor-agent` returns 0 because the process name is `node`.

Inspect recent tool calls with:

```bash
jq -r 'select(.type=="tool_call" and .subtype=="started") | .tool_call
       | to_entries[0] | "\(.key) \(.value.args.command // .value.args.path // "")"' \
  events.jsonl | tail -3
```

## Driven from a Claude Code orchestrator

- Launch each run through a launcher script called with Bash `run_in_background`, and have the script write a completion file (for example `<name>.done`) last.
- An interactive session is re-invoked by the background-task notification. `claude -p` is not re-invoked after `end_turn`, so block the same turn on the completion files: `until [ -f "$run_dir/$name.done" ]; do sleep 5; done`. A Bash call stops at 600 s, so repeat the wait in further calls for longer runs.
- Claude's auto-mode classifier may refuse a launch whose prompt asks Cursor to run shell commands or install hooks ("Create Unsafe Agents"). Add an allow rule for the launcher script, such as `Bash(bash scripts/<launcher>.sh:*)` in `.claude/settings.json`, rather than rewording the prompt until it passes.
