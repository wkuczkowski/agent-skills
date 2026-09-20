# Sessions, multitasking, and monitoring

Read this file when a Cursor task needs follow-up turns, subagents, background monitoring, many independent runs, or is driven from a Claude Code orchestrator.

## Sessions

Every run in SKILL.md already starts from `sid=$(cursor-agent create-chat)`. A follow-up turn is the same command with a new prompt file:

```bash
cursor-agent -p --trust --auto-review --sandbox disabled \
  --model cursor-grok-4.6-high --resume "$sid" \
  --output-format stream-json < followup.txt >events-2.jsonl 2>err-2.log
```

- A resumed run keeps the session id, and the results of every completed tool call, MCP ones included, stay in context. Tool calls and subagents that were in flight when a run was killed return as "interrupted" with no content.
- Resume by id. `--continue` takes the most recent session on the machine, which under parallel runs is someone else's.
- `--output-format json` prints nothing until the run ends, so a killed run leaves no output at all. Use `stream-json`.

## Subagents

Grok delegates to Task subagents on its own when a task is broad; `/multitask` at the start of the prompt only makes it more likely. Subagents stay in-process and do not start another `cursor-agent`.

- To keep the work in the parent, end the prompt with: "Do the work yourself; do not delegate to Task subagents." The agent complied in every measured run.
- When delegating on purpose, give each subagent a self-contained task with absolute paths.
- Subagents run on the parent's slug only with the client configuration named in SKILL.md; verify it in the stream.
- The parent stream is silent while subagents work (measured 2.5–4 minutes each). Their own tool calls do not appear in it.
- A `taskToolCall` `started` event carries `args.agentId`, `args.model`, `args.subagentType` and the prompt. Its `completed` event means the subagent finished; the report is in `result.success.conversationSteps`.
- A subagent cannot be resumed or inspected afterwards: `--resume <agentId>` opens an empty session. A subagent's work survives only through the parent's `completed` event or a file it wrote.

List subagents with model and duration in ms (`running` until completed):

```bash
jq -r 'select(.type=="tool_call" and .tool_call.taskToolCall) | .tool_call.taskToolCall
       | [.args.agentId[0:8], .args.model, .args.description,
          (.result.success.durationMs // "running")] | @tsv' events.jsonl
```

## Parallel runs

Launch mass parallel runs from an external runner: a bash or Python script that starts N independent `cursor-agent -p` processes, each with its own `create-chat` id, prompt file and stream. Three concurrent research runs on one account finished without interference. Auto Review blocks a nested `cursor-agent` launched from inside a `--auto-review` session: the child never starts, and the parent does the work itself.

```bash
sid=$(cursor-agent create-chat); echo "$sid" > "$run_dir/$name.sid"
nohup timeout -k 15 3600 cursor-agent -p --trust --auto-review --sandbox disabled \
  --workspace "$PWD" --model cursor-grok-4.6-high --resume "$sid" --output-format stream-json \
  < "$run_dir/$name.prompt" > "$run_dir/$name.jsonl" 2> "$run_dir/$name.err" &
```

## Monitoring

- Completion is a final `result` event and exit 0.
- A `tool_call` event with `subtype: started` and no matching completion is in flight. A `completed` one carries `startedAtMs` and `completedAtMs`.
- Seconds since the stream last grew: `echo $(( $(date +%s) - $(stat -c %Y events.jsonl) ))`. Past 600, treat the run as hung (see Time in SKILL.md). `--stream-partial-output` adds text deltas when the caller needs them.
- Count live runs with `pgrep -fc "cursor-agent/versions/.*index\.js -p"`. Looser patterns also match the `timeout` wrapper and the calling shell; `pgrep -c cursor-agent` returns 0 because the process name is `node`.
- Each web search adds an `interaction_query` request and response pair; the response holds `approved` or `rejected`.
- The stream is the full record. Cursor's own transcript under `~/.cursor/projects/<slug>/agent-transcripts/` keeps no tool results, so keep the jsonl.

Inspect recent tool calls with:

```bash
jq -r 'select(.type=="tool_call" and .subtype=="started") | .tool_call
       | to_entries[0] | "\(.key) \(.value.args.command // .value.args.path // "")"' \
  events.jsonl | tail -3
```

## Driven from a Claude Code orchestrator

- Launch each run through a launcher script called with Bash `run_in_background`, and have the script write a completion file (for example `<name>.done`, holding the exit code) last.
- An interactive session is re-invoked by the background-task notification. `claude -p` is not re-invoked after `end_turn`, so block the same turn on the completion files: `until [ -f "$run_dir/$name.done" ]; do sleep 5; done`. A Bash call stops at 600 s, so repeat the wait in further calls for longer runs.
- Claude's auto-mode classifier may refuse a launch ("Create Unsafe Agents"). On this machine the user's `~/.claude/settings.json` allows commands that begin with `cursor-agent`, `timeout … cursor-agent`, `nohup cursor-agent` and `nohup timeout … cursor-agent`, so begin the command that way. If a launch is still refused, ask the user for an allow rule rather than rewording the prompt until it passes.
