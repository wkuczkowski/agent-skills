---
name: cursor-headless
description: Run Cursor Agent CLI programmatically via cursor-agent -p. Use when delegating a task to a Cursor agent, scripting cursor-agent in shell or CI, or when the user mentions running Cursor headless or programmatically.
---

# Cursor headless (`cursor-agent -p`)

Drive Cursor Agent CLI non-interactively. The commands below were checked with `cursor-agent 2026.09.02-c22c1a3`.

## Core call

```bash
cursor-agent -p --trust --auto-review --sandbox disabled "<task>" \
  --model cursor-grok-4.6-high-fast --output-format json 2>err.log > out.json
```

- stdout carries exactly one JSON object — keep stderr redirected away from it.
- Read from the JSON: `.result` (final answer), `.session_id`, `.is_error`, `.subtype` (`success`), `.usage` (token counts), `.request_id`.
- Pipe input via stdin: `echo "data" | cursor-agent -p --trust --auto-review --sandbox disabled --model cursor-grok-4.6-high-fast "<prompt about the piped input>"`. Stdin is appended to the prompt argument.
- Exit codes: `0` success, `1` error (bad model, missing trust, auth failure) with the reason on stderr. Check both the exit code and `.is_error`.
- **`--trust` is mandatory headless.** Without it the run dies with exit 1 and a "Workspace Trust Required" prompt on stderr.
- Install: `curl https://cursor.com/install -fsS | bash` (lands in `~/.local/bin` as `cursor-agent` and `agent`). Update with `cursor-agent update`. Auth: `cursor-agent login` (browser) or `CURSOR_API_KEY` env var for CI.

## Model

Always use `cursor-grok-4.6-high-fast` (Cursor Grok 4.6 High Fast). The slug fixes reasoning effort at `high` and enables Fast serving. Keep this model for every task unless the user explicitly requests another one.

- `cursor-agent models` lists every valid slug for the account (also printed in the error when you pass a bad `--model`).
- The `system/init` event in stream-json must confirm the resolved model display name `Cursor Grok 4.6 High Fast`.

## Permissions and approvals

- Always pass `--auto-review --sandbox disabled`. Keep both flags in scripts even when the global config has the same values, so automation does not depend on interactive settings.
- Auto-review sends Shell, MCP, and Fetch calls through Cursor's classifier. It may reject an operation or require human approval. Report that result instead of retrying with `--force`, `--yolo`, or `unrestricted`.
- Auto-review is a convenience guardrail, not isolation. On this workstation the native Cursor sandbox is disabled because its AppArmor preflight fails. Keep access scoped through the task, workspace, CLI permissions, and the surrounding execution environment.
- CLI permissions live in `.cursor/cli.json` for a project or `~/.cursor/cli-config.json` globally. Both `allow` and `deny` arrays are required. Keep the allowlist narrow because allowlisted calls bypass classifier review:

  ```json
  {
    "version": 1,
    "editor": {"vimMode": false},
    "approvalMode": "auto-review",
    "permissions": {"allow": ["Shell(ls)"], "deny": []},
    "sandbox": {"mode": "disabled", "networkAccess": "user_config_with_defaults"}
  }
  ```

  Rule syntax: `Shell(cmd)`, `Read(glob)`, `Write(glob)`, `WebFetch(domain)`, `Mcp(server:tool)`.
- `permissions.json` can steer the Auto-review classifier with plain-language `allow_instructions` and `block_instructions`. Add it only for recurring rules. It is guidance, not a security boundary.
- If Cursor fixes the AppArmor preflight, test `--sandbox enabled` before changing this default. A working sandbox can then use `sandbox.json` for narrow network and filesystem exceptions.
- `--approve-mcps` auto-approves configured MCP servers. Use it only when the task requires those servers.
- Restrict to `--mode plan` or `--mode ask` (read-only) only when the task demands it: untrusted inputs, pure analysis, audits.

## MCP servers

Getting an MCP server working headless takes three pieces — all verified end-to-end (a stdio server was configured, auto-approved, and its tools called from `-p` with zero prompts):

1. **Server config** — project `.cursor/mcp.json` (or global `~/.cursor/mcp.json`):

   ```json
   {"mcpServers": {"my-server": {"type": "stdio", "command": "/abs/path/to/cmd", "args": ["--directory", "/abs/path/to/project", "..."]}}}
   ```

   **Use absolute paths only.** IDE variables like `${userHome}` and `${workspaceFolder}` are NOT interpolated by the CLI — the server silently fails with "Connection failed" in `cursor-agent mcp list`.

2. **Server approval (one-time per repo)** — a configured server still sits at "not loaded (needs approval)". Approve it once from the repo root: `cursor-agent mcp enable my-server`. Per-run alternative for scripts: pass `--approve-mcps`. Without either, the agent reports an empty MCP tool catalog.

3. **Tool-call auto-approval** — so individual tool calls never wait for verification, add an allow rule to `.cursor/cli.json` (remember: both arrays required):

   ```json
   {"permissions": {"allow": ["Mcp(my-server:*)"], "deny": []}}
   ```

Diagnostics: `cursor-agent mcp list` (per-repo status — expect `my-server: ready`), `cursor-agent mcp list-tools my-server` (connects and lists tools with argument names). Check these before blaming the prompt when the agent says it has no MCP tools.

## Sessions

```bash
sid=$(cursor-agent -p --trust --auto-review --sandbox disabled --model cursor-grok-4.6-high-fast \
  "Analyze X" --output-format json | jq -r .session_id)
cursor-agent -p --trust --auto-review --sandbox disabled --model cursor-grok-4.6-high-fast \
  --resume "$sid" "Follow-up: ..." --output-format json | jq -r .result
```

Resumed sessions keep full context (verified: recalled the output of a command run in the earlier turn). `--continue` resumes the most recent session when you did not capture the id; `cursor-agent ls` lists sessions.

## Multitasking (parallel subagents)

Cursor's multitasking — one parent agent spawning parallel subagents, each in its own context window — **works headless**. Verified: two subagents ran concurrently, each got its own chat id, and the parent waited for both and combined their reports.

### Spawning

Prefix the prompt with `/multitask` and be explicit about the decomposition — for a trivial task the model skips subagents and just does the work itself with direct edits:

```bash
cursor-agent -p --trust --auto-review --sandbox disabled "/multitask Use parallel subagents: subagent 1 <task A>; subagent 2 <task B>. Combine both reports." \
  --model cursor-grok-4.6-high-fast --output-format stream-json > stream.jsonl 2>/dev/null &
```

Give each subagent a self-contained brief (absolute paths included) — the parent forwards your decomposition nearly verbatim as each subagent's prompt.

### Tracking subagent progress

Everything arrives on the parent's stream-json; subagents' own tool calls are not streamed, so you track them via spawn events, the parent's narration, and post-hoc interrogation:

- **What was spawned** — `tool_call` events with a `taskToolCall` key carry each subagent's `.args.description` and full `.args.prompt`:
  ```bash
  jq -r 'select(.type=="tool_call" and .subtype=="started") | .tool_call.taskToolCall | select(.) | .args.description' stream.jsonl
  ```
- **`taskToolCall completed` ≠ subagent finished.** It fires within milliseconds and only confirms the launch (`.result.success.agentId`, `isBackground: true`). Grab the `agentId` from it — that is the subagent's chat id:
  ```bash
  jq -r 'select(.type=="tool_call" and .subtype=="completed") | .tool_call.taskToolCall | select(.)
         | .args.description + " " + (.result | fromjson | .success.agentId)' stream.jsonl
  ```
- **Which subagents are done** — the parent narrates completions in its `assistant` events and the final `result` names each subagent with its chat id ("[Count .txt lines](e8ede733-…) has completed").
- **Interrogate a subagent** — subagent chats are resumable like any session (verified): `cursor-agent -p --trust --auto-review --sandbox disabled --model cursor-grok-4.6-high-fast --resume <agentId> "What did you find?"`. Use this after the run to pull details the parent's summary dropped, or mid-run to peek at a slow subagent.
- Stuck-detection is the same as for a single run: if `stream.jsonl` stops growing for minutes while a `taskToolCall` spawn has no matching completion narration, the parent is stalled waiting on that subagent.

## Monitoring a background run

For long tasks, launch in the background with stream-json going to a file, then poll the file — each line is a JSONL event:

```bash
cursor-agent -p --trust --auto-review --sandbox disabled "<long task>" --model cursor-grok-4.6-high-fast \
  --output-format stream-json > stream.jsonl 2>/dev/null &
```

- Event types: `system/init` (session_id, model, permissionMode), `user`, `assistant`, `thinking` (delta/completed), `tool_call` (started/completed), final `result`.
- **What is it doing right now** — the most recent tool calls (a `started` without a matching `completed` is in flight; shell calls carry the exact command):
  ```bash
  jq -r 'select(.type=="tool_call" and .subtype=="started") | .tool_call | to_entries[0]
         | "\(.key) \(.value.args.command // .value.args.path // "")"' stream.jsonl | tail -3
  ```
- **Is it stuck** — check the file's mtime: if `stream.jsonl` stops growing for minutes, the agent is stalled at its last `tool_call started`.
- **Is it done** — the final line is a `result` event with the same fields as `--output-format json` (`.result`, `.session_id`, `.is_error`).
- **Follow up** — after completion, interrogate or steer the same agent with `--model cursor-grok-4.6-high-fast --resume <session_id>`.
- `--stream-partial-output` adds per-token text deltas if you need live output.

## Gotchas

- **No structured-output schema flag.** Unlike `claude --json-schema` / `codex --output-schema`, output formats are only `text | json | stream-json`; if you need JSON data, ask for it in the prompt and parse `.result`.
- **Put absolute paths in the prompt** and verify artifacts exist after the call — a blocked tool call in `-p` mode surfaces only in the agent's prose.
- **Set generous timeouts.** A trivial question takes ~10 s; real work takes minutes.
- Outside a git repo the CLI still works; `--workspace <path>` sets the working root.
