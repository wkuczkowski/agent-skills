# Hooks

Read this file before adding a Cursor hook, or when a run behaves as if something outside the prompt acted on it.

## Configure

Project hooks live in `.cursor/hooks.json` and load only in a trusted workspace; that level is the one measured in headless mode. The CLI bundle also names user (`~/.cursor/hooks.json`), team and enterprise (`/etc/cursor/hooks.json`) levels, not tested here.

```json
{"version": 1, "hooks": {"postToolUse": [{"command": "/abs/path/to/script postToolUse"}]}}
```

A project hook is not tied to headless runs; it would also fire in Cursor sessions a person opens by hand in that repository, so scope it to the runs it is meant for.

## Events

The CLI bundle names `sessionStart`, `sessionEnd`, `beforeSubmitPrompt`, `preToolUse`, `postToolUse`, `postToolUseFailure`, `beforeShellExecution`, `afterShellExecution`, `beforeMCPExecution`, `afterMCPExecution`, `beforeReadFile`, `afterFileEdit`, `afterAgentResponse`, `stop`, `subagentStart` and `subagentStop`.

Measured 2026-09-19 on a headless run that read one file, with a hook on every event: `sessionStart`, `preToolUse`, `beforeReadFile`, `postToolUse` and `sessionEnd` fired; `stop` and `afterAgentResponse` did not. The shell, MCP, edit and subagent events are unverified.

## Payloads

- `postToolUse` carries `tool_name`, `tool_input` (a JSON string), `tool_output`, `duration` in ms, `tool_use_id`, `conversation_id`, `model` and `transcript_path`.
- `sessionEnd` carries `reason` (`completed`), `duration_ms` and `final_status`.
- Every payload carries `user_email`. Never write raw payloads to a committed trace or log.
- A hook sees only Cursor's `conversation_id`, not a name the caller gave the run. To attribute tool calls to a run, parse its stream-json instead (see [monitoring](sessions-multitasking-monitoring.md#monitoring)).
