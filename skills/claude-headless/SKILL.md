---
name: claude-headless
description: Run Claude Code programmatically via the headless CLI (claude -p). Use when delegating a task to a separate Claude Code instance, scripting claude in shell or CI, or when the user mentions running Claude Code headless or programmatically.
---

# Claude Code headless (`claude -p`)

Workflow for driving Claude Code from a script or from another agent. Everything below was verified by actually running it.

## Core call

```bash
claude -p "<task>" --allowedTools "Read,Glob,Grep,Write,Edit" --permission-mode auto \
  --output-format json 2>err.log > out.json
```

- stdout carries exactly one JSON object — keep stderr redirected away from it.
- Read from the JSON: `.result` (final answer), `.session_id`, `.is_error`, `.num_turns`, `.permission_denials`, `.total_cost_usd`.
- Pipe input via stdin: `git diff | claude -p "review this diff"`.
- Exit codes: `0` success, `1` error or `--max-turns` hit, `143` killed. Exit `0` with `.is_error: true` happens (e.g. auth failure) — always check both.

## Gotchas

- **`--bare` breaks claude.ai subscription auth** — it fails instantly with "Not logged in" even when `claude auth status` says logged in. Skip `--bare` unless using API-key auth.
- **Put absolute paths in the prompt.** The child session has its own scratchpad; "create a file here" can land there instead of your cwd. Name explicit absolute target paths and verify the artifact exists after the call.
- **`--json-schema` output is a JSON *string* inside `.result`** — parse it: `jq -r '.result | fromjson'`.
- **Set generous timeouts.** A trivial repo question takes ~20 s; real work takes minutes.

## Multi-turn collaboration

Resumed sessions keep full context and answer much faster (no re-exploration):

```bash
sid=$(claude -p "Analyze X in this repo" --output-format json | jq -r .session_id)
claude -p "Follow-up: ..." --resume "$sid" --output-format json | jq -r .result
```

`--continue` resumes the most recent session in the cwd when you did not capture the id.

## Monitoring a background run

For long tasks, launch in the background with the event stream going to a file, then poll the file — each line is a JSONL event:

```bash
claude -p "<long task>" --allowedTools "Read,Glob,Grep,Write,Edit" --permission-mode auto \
  --output-format stream-json --verbose > stream.jsonl 2>/dev/null &
```

- **What is it doing right now** — the most recent `tool_use` events:
  ```bash
  jq -r 'select(.type=="assistant") | .message.content[]? | select(.type=="tool_use")
         | "\(.name) \(.input.file_path // .input.command // .input.pattern // "")"' stream.jsonl | tail -3
  ```
- **Is it stuck** — check the file's mtime: if `stream.jsonl` stops growing for minutes, the agent is stalled, and its last `tool_use` shows exactly where.
- **Is it done** — the final line is a `result` event with the same fields as `--output-format json` (`.result`, `.session_id`, `.is_error`).
- **Follow up** — after completion, interrogate or steer the same agent with `--resume <session_id>`.

Verify deliverables yourself (the file exists, tests pass) rather than trusting the final message alone.

## Permissions

- **Default: `--permission-mode auto` with write access.** `--allowedTools "Read,Glob,Grep,Write,Edit" --permission-mode auto` — an agent that can create and edit files is far more useful, and `auto` removes routine prompts while a background classifier still blocks genuinely dangerous actions (`curl | bash`, force-push, production deploys, `rm -rf /`). Requires Plan/Team/Enterprise and a recent model (Sonnet 4.6+/Opus 4.6+/Fable 5). A blocked Write in `-p` mode fails silently, leaving the agent to describe work instead of doing it.
- Narrower fallbacks: `acceptEdits` auto-approves only file edits + basic FS commands; `dontAsk` denies everything outside `permissions.allow` — the docs' pick for locked-down CI.
- Restrict to read-only (`--allowedTools "Read,Glob,Grep"`) only in the rare cases that demand it: untrusted prompts/inputs, pure analysis of a repo that must stay pristine, audits.
- Scope Bash by prefix pattern: `--allowedTools "Bash(git diff *),Bash(ls *)"`.
- Tools outside the allowlist are silently denied in `-p` mode — check `.permission_denials` when the agent reports it could not do something.
- Reserve `--dangerously-skip-permissions` for disposable sandboxes.

## MCP servers

Verified end-to-end: a project `.mcp.json` at the repo root is loaded automatically in `-p` mode, and allowlisting the server makes its tool calls run with zero approval prompts.

```bash
claude -p "<task using the MCP tools>" --allowedTools "mcp__my-server" --permission-mode auto --output-format json
```

- `.mcp.json` shape: `{"mcpServers": {"my-server": {"command": "/abs/path/cmd", "args": ["..."]}}}`. Absolute command paths are safest; relative `args` paths resolve against the cwd.
- `--allowedTools "mcp__<server>"` allowlists **every** tool on that server; `mcp__<server>__<tool>` narrows to one tool. Without the allowlist entry MCP calls are silently denied like any other tool (check `.permission_denials`).
- Extra servers ad hoc: `--mcp-config <file>`; `--strict-mcp-config` ignores all other MCP sources.

## Structured output

```bash
git log --oneline -10 | claude -p "Classify each commit by conventional-commit type" \
  --output-format json \
  --json-schema '{"type":"object","properties":{"commits":{"type":"array","items":{"type":"object","properties":{"hash":{"type":"string"},"type":{"type":"string"}},"required":["hash","type"]}}},"required":["commits"]}' \
  | jq -r '.result | fromjson'
```

## Streaming and applications

- Live progress events: `--output-format stream-json --verbose --include-partial-messages` (newline-delimited JSON).
- Building an application rather than a shell script → use the Claude Agent SDK instead of the CLI: Python `uv add claude-agent-sdk`, TypeScript `npm install @anthropic-ai/claude-agent-sdk`. Docs: https://code.claude.com/docs/en/agent-sdk/overview
