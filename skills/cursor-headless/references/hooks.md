# Hooks

Read before adding a Cursor hook, or when a run behaves as if something outside the prompt acted on it. Event names and the config format are in Cursor's hooks docs; the hook event set in the 2026.10.22 bundle is the same as in 2026.10.01.

- Project hooks live in `.cursor/hooks.json` and load only in a trusted workspace; that level is the one measured headless. User (`~/.cursor/hooks.json`) and enterprise (`/etc/cursor/hooks.json`) levels were not tested.
- A project hook also fires in Cursor sessions a person opens by hand in that repository, so scope it to the runs it is meant for.
- Measured 2026-09-19 (2026.09.18) on a headless run that read one file, with a hook on every event: `sessionStart`, `preToolUse`, `beforeReadFile`, `postToolUse` and `sessionEnd` fired; `stop` and `afterAgentResponse` did not. Shell, MCP, edit and subagent events were not measured.
- Since 2026.10.17 (changelog): `postToolUse` and `postToolUseFailure` matching `Task` fire when a subagent finishes or fails, a hook's `additional_context` reaches the main agent, and hooks inside a subagent receive its transcript path.
- `postToolUse` carries `tool_name`, `tool_input` (a JSON string), `tool_output`, `duration` (ms), `tool_use_id`, `conversation_id`, `model` and `transcript_path`; `sessionEnd` carries `reason`, `duration_ms` and `final_status`.
- Every payload carries `user_email`. Never write raw payloads to a committed trace or log.
- A hook sees Cursor's `conversation_id`, not a name the caller gave the run; to attribute tool calls to a run, parse its stream-json.
