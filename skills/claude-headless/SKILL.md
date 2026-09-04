---
name: claude-headless
description: Run Claude Code programmatically via the headless CLI (claude -p). Use when delegating a task to a separate Claude Code instance, scripting claude in shell or CI, or when the user mentions running Claude Code headless or programmatically.
---

# Claude Code headless (`claude -p`)

Drive Claude Code non-interactively. The commands below were checked with Claude Code `2.1.260`.

## Core call

```bash
claude -p "<task>" \
  --model fable --effort high \
  --permission-mode auto --permission-prompts none \
  --strict-mcp-config \
  --output-format json 2>err.log >out.json
```

- Always pass `--model fable --effort high`. The alias follows the newest Fable release. It currently resolves to Claude Fable 5.1 with a 1M context window.
- Always pass `--permission-mode auto --permission-prompts none`. Auto mode reviews tool calls with a separate classifier. The second flag makes unattended runs deny unresolved prompts instead of waiting for a person.
- Use `--strict-mcp-config` when the task needs no MCP server. It prevents user connectors and a repository's `.mcp.json` from entering an otherwise deterministic run.
- Successful `--output-format json` writes one JSON object to stdout. Read `.result`, `.session_id`, `.is_error`, `.subtype`, `.terminal_reason`, `.num_turns`, `.permission_denials`, `.usage`, `.modelUsage`, and `.total_cost_usd`.
- Inspect the process exit code, stderr, `.is_error`, and `.terminal_reason`. CLI and sandbox preflight errors can exit before producing JSON. In-run failures can still produce a JSON result.
- Piped stdin is appended to the prompt and is limited to 10 MB. Put larger inputs in a file and name its absolute path in the prompt.
- Add `--no-session-persistence` for one-shot tasks that do not need `--resume`. This avoids writing the transcript under `~/.claude/projects`.

## Authentication and updates

- Check the installation with `claude --version`, `claude doctor`, and `claude auth status`.
- Native installations update in the background. `claude update` applies an update immediately.
- Claude.ai login works in normal print mode. `--bare` does not read OAuth or keychain credentials, so use it only with `ANTHROPIC_API_KEY`, `apiKeyHelper`, or a supported cloud provider.

## Permissions

- Keep Auto mode enabled for every headless run. A blocked action is a result to report or solve safely. Do not retry it with `bypassPermissions` or `--dangerously-skip-permissions`.
- `--allowedTools` adds pre-approval rules. It does not remove unlisted tools. Auto mode already approves reads and ordinary edits inside the working directory, so a broad `--allowedTools` list adds little value and can bypass classifier checks.
- Use `--tools` or a bare `--disallowedTools ToolName` entry to remove a tool from the model. Use scoped deny entries such as `--disallowedTools "Bash(rm *)"` to block matching calls while keeping the rest of Bash available.
- For a fixed allowlist, use `--permission-mode dontAsk` with narrow `--allowedTools` rules. Use `--permission-mode plan` for read-only planning.
- Auto mode can stop after repeated classifier blocks because no person can answer in print mode. Check `.permission_denials` and the final result before treating the task as complete.

The matching global baseline in `~/.claude/settings.json` is:

```json
{
  "model": "fable",
  "effortLevel": "high",
  "permissions": {"defaultMode": "auto"},
  "sandbox": {
    "enabled": true,
    "autoAllowBashIfSandboxed": true,
    "failIfUnavailable": true,
    "allowUnsandboxedCommands": true
  }
}
```

Preserve existing deny rules when adding this baseline.

## Sandbox

Claude's sandbox covers Bash commands and their child processes. Built-in Read, Edit, and Write tools remain governed by permissions.

- On Linux, sandboxing requires `bubblewrap`, `socat`, and a working AppArmor user-namespace profile for `bwrap`.
- Sandboxed commands can write inside the working directory. New network destinations go through the normal permission flow and Auto classifier.
- `autoAllowBashIfSandboxed: true` removes prompts for commands that remain inside the sandbox.
- `failIfUnavailable: true` stops the run if isolation cannot start. This avoids a silent unsandboxed fallback.
- `allowUnsandboxedCommands: true` lets Claude request a retry outside the sandbox for an incompatible command. Auto mode still reviews that request. With `--permission-prompts none`, unresolved requests are denied rather than waiting.
- Add `sandbox.filesystem.allowWrite`, network domains, or `excludedCommands` only for a concrete task. Keep each exception narrow.

## MCP servers

Default headless runs use `--strict-mcp-config` with no MCP config. When a task needs MCP, pass only the required configuration:

```bash
claude -p "<task using MCP>" \
  --model fable --effort high \
  --permission-mode auto --permission-prompts none \
  --strict-mcp-config --mcp-config /absolute/path/mcp.json \
  --allowedTools "mcp__my-server__*" \
  --output-format json
```

- MCP tool names use `mcp__<server>__<tool>`. Use `mcp__<server>__*` for every tool on one explicitly selected server.
- Prefer absolute command and argument paths in stdio server definitions.
- Without `--strict-mcp-config`, Claude loads account connectors and repository MCP configuration. A project `.mcp.json` can start a local command in print mode without an interactive trust dialog.

## Structured output

```bash
claude -p "Extract function names from auth.py" \
  --model fable --effort high \
  --permission-mode auto --permission-prompts none \
  --strict-mcp-config --output-format json \
  --json-schema '{"type":"object","properties":{"functions":{"type":"array","items":{"type":"string"}}},"required":["functions"]}' \
  | jq '.structured_output'
```

Read validated data from `.structured_output`. `.result` may contain a textual rendering of the same value, but it is not the structured-output field.

## Sessions

```bash
sid=$(claude -p "Analyze X" \
  --model fable --effort high \
  --permission-mode auto --permission-prompts none \
  --strict-mcp-config --output-format json | jq -r .session_id)

claude -p "Follow-up: ..." --resume "$sid" \
  --model fable --effort high \
  --permission-mode auto --permission-prompts none \
  --strict-mcp-config --output-format json | jq -r .result
```

`--continue` resumes the most recent session for the current directory. Do not combine resumable work with `--no-session-persistence`.

## Limits and monitoring

- `--max-turns N` limits tool-use round trips. `--max-budget-usd X` stops when the client-side cost estimate reaches the limit. Use them for CI and other bounded automation, but size them for the task.
- Claude Code has no general wall-clock timeout flag for print mode. The parent process must enforce one. Send SIGTERM for a controlled stop; the process exits 143 and a persisted session can be resumed.
- For live progress, use `--output-format stream-json --verbose`. Add `--include-partial-messages` for token deltas and `--forward-subagent-text` when the caller needs subagent text.
- The last JSONL record is the terminal `result` event. Track the latest assistant `tool_use` event and the file's modification time to detect a stalled run.
- Shell backgrounding remains valid for print mode:

  ```bash
  claude -p "<long task>" \
    --model fable --effort high \
    --permission-mode auto --permission-prompts none \
    --strict-mcp-config --output-format stream-json --verbose \
    >stream.jsonl 2>err.log &
  ```

Native `claude --bg` sessions are a separate workflow managed with `claude agents`, `claude logs`, `claude attach`, `claude stop`, and `claude rm`. They do not combine with `-p`.

## Data handling

- Claude Max is a consumer account. Fable 5.1 requires at least 30 days of server-side retention. If Model Improvement is enabled in Claude.ai, consumer data may be retained for up to five years.
- Local persisted transcripts are plaintext under `~/.claude/projects` and default to 30-day cleanup.
- Use `--no-session-persistence` when resumption is unnecessary. Follow the host's data policy and keep client or regulated data out of consumer Claude sessions.
- Use the Claude Agent SDK instead of shell parsing when building an application around the agent loop.
