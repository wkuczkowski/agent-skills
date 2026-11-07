# Sessions, subagents, parallel runs, monitoring

Read when a Cursor task needs follow-up turns, subagents, many runs, monitoring, or is driven from a Claude Code orchestrator. Measured on 2026.09.18 unless dated otherwise; 2026.10.22 changelog items are marked.

## Sessions

A follow-up turn is the SKILL.md command with the same `--resume "$sid"` and a new prompt file and stream.

- A resumed run keeps the session and every completed tool result, MCP ones included; since 2026.10.17 completed tool calls do not rerun on resume. Tool calls and subagents in flight when the run was killed come back as "interrupted" with no content.
- Resume by id. `--continue` takes the most recent session on the machine, which under parallel runs is another run's.

## Subagents

Grok delegates to in-process Task subagents on its own when a task is broad.

- To keep the work in the parent, end the prompt with "Do the work yourself; do not delegate to Task subagents." It complied in every measured run.
- When delegating on purpose, give each subagent a self-contained task with absolute paths.
- Subagents run on the parent's slug only with the client configuration in SKILL.md; check `args.model` in the stream.
- The parent stream is silent while subagents work, and their tool calls do not appear in it.
- A `taskToolCall` `started` event carries `args.agentId`, `args.model`, `args.subagentType` and the prompt; `completed` means the subagent finished, with its report in `result.success.conversationSteps` (an object, not a JSON string). Since 2026.10.15/10.22 a failed or stopped subagent reports its id and reason to the parent.
- `cursor-agent --resume <agentId>` opened an empty session (2026.09.18); a subagent's work survived only through the parent's `completed` event or a file it wrote. The 2026.10.15 changelog says resuming a failed subagent keeps its type and model, which likely means a resume from inside the parent session; the CLI form was not re-tested.

List subagents with model and duration in ms:

```bash
jq -r 'select(.type=="tool_call" and .tool_call.taskToolCall) | .tool_call.taskToolCall
       | [.args.agentId[0:8], .args.model, .args.description,
          (.result.success.durationMs // "running")] | @tsv' events.jsonl
```

## Parallel runs

Start N independent `cursor-agent -p` processes from an external script, each with its own `create-chat` id, prompt file and stream; three concurrent runs on one account did not interfere. A nested `cursor-agent` launched from inside an `--auto-review` session is blocked by Auto Review: the child never starts.

## Monitoring

- A `tool_call` `started` with no matching `completed` is in flight; `completed` carries `startedAtMs` and `completedAtMs`.
- Seconds since the stream last grew: `echo $(( $(date +%s) - $(stat -c %Y events.jsonl) ))`. Judge a hang by silence, not elapsed time; see [observed in use](observed-in-use.md) for the pause lengths seen.
- Count live runs with `pgrep -fc "cursor-agent/versions/.*index\.js -p"`. The process is `node` (so `pgrep -c cursor-agent` returns 0), and looser patterns also match the `timeout` wrapper and the calling shell. The launcher still execs `node .../versions/<ver>/index.js` on 2026.10.22.
- Each web search adds an `interaction_query` `request`/`response` pair; the response holds `approved` or `rejected`.
- Keep the jsonl: Cursor's own transcript under `~/.cursor/projects/<slug>/agent-transcripts/` has no tool results.

Recent tool calls:

```bash
jq -r 'select(.type=="tool_call" and .subtype=="started") | .tool_call
       | to_entries[0] | "\(.key) \(.value.args.command // .value.args.path // "")"' \
  events.jsonl | tail -3
```

## Driven from a Claude Code orchestrator

- Launch each run through a script called with Bash `run_in_background` that writes a completion file (for example `<name>.done` holding the exit code) last.
- An interactive Claude Code session is re-invoked by the background-task notification; `claude -p` is not re-invoked after `end_turn`, so block the same turn on the completion files (`until [ -f "$run_dir/$name.done" ]; do sleep 5; done`), repeating the wait across Bash calls, which stop at 600 s.
- Claude's auto-mode classifier refused a direct launch ("Create Unsafe Agents", 2026-09-20) until the user added allow rules to `~/.claude/settings.json`. On 2026-11-07 that file allows `Bash(cursor-agent:*)` and `Bash(nohup cursor-agent *)` only; the `timeout … cursor-agent` rules from September are gone, so a command starting with `timeout` may be classified. If a launch is refused, ask the user for a rule rather than rewording the prompt until it passes.
