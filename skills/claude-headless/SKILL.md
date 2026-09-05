---
name: claude-headless
description: Use when you need Anthropic's Fable model and are working outside the Claude Code harness. Runs Claude Code headlessly with high reasoning effort.
---

# Claude Code headless (`claude -p`)

Drive Claude Code non-interactively. These instructions target Claude Code `2.1.260`.

## Choose a run

Use this for a one-shot task:

```bash
claude -p "<task>" \
  --model fable --effort high \
  --permission-mode auto --permission-prompts none \
  --strict-mcp-config --no-session-persistence \
  --output-format json 2>err.log >out.json
```

- Always pass `--model fable --effort high`.
- Always pass `--permission-mode auto --permission-prompts none`.
- Use `--strict-mcp-config` when the task needs no MCP server. It excludes account connectors and repository MCP configuration.
- Remove `--no-session-persistence` when the task may need `--resume`.
- For resumable or long-running work, read [sessions and monitoring](references/sessions-and-monitoring.md).
- For MCP or schema-validated output, read [MCP and structured output](references/mcp-and-structured-output.md).

If the CLI itself fails, hangs, selects an unavailable model, emits malformed output, or cannot initialize its sandbox or permissions, read [problem reporting](references/problem-reporting.md). Record the sanitized failure, tell the user where the report is, and continue with unaffected work.

## Output and completion

- Successful JSON mode writes one object to stdout. Inspect the process exit code, stderr, `.is_error`, `.subtype`, `.terminal_reason`, and `.permission_denials`.
- Completion requires exit `0`, `.is_error: false`, `.subtype: success`, and `.terminal_reason: completed`.
- Read the final text from `.result`. Other useful fields include `.session_id`, `.num_turns`, `.usage`, `.modelUsage`, and `.total_cost_usd`.
- `.modelUsage` normally contains `claude-fable-5-1` for the task and a smaller classifier model such as `claude-haiku-4-5` for Auto mode. The classifier entry does not mean the task switched away from Fable.
- CLI and sandbox preflight errors can exit before producing JSON. An in-run failure may still produce a JSON result.
- Piped stdin is appended to the prompt and is limited to 10 MB. Put larger inputs in a file and name its absolute path.

## Authentication and updates

- Check the installation with `claude --version`, `claude doctor`, and `claude auth status`.
- Native installations update in the background. `claude update` applies an update immediately.
- Claude.ai login works in normal print mode. `--bare` ignores OAuth and keychain credentials, so use it only with `ANTHROPIC_API_KEY`, `apiKeyHelper`, or a supported cloud provider.

## Permissions

- Keep Auto mode enabled for every run. A blocked action is a result to report or solve within the existing controls.
- `--allowedTools` adds pre-approval rules. It does not remove unlisted tools and can bypass classifier checks.
- Use `--tools` or a bare `--disallowedTools ToolName` entry to remove a tool. Use a scoped deny such as `--disallowedTools "Bash(rm *)"` to block matching calls while keeping other Bash calls.
- Use `dontAsk` with narrow allow rules for a fixed allowlist. Use `plan` for read-only planning.
- Do not retry with `bypassPermissions` or `--dangerously-skip-permissions`.

## Sandbox

Inherit Claude Code's existing sandbox settings when launching `claude -p`. The user's default is sandboxing disabled. Keep sandbox configuration out of launch flags and `--settings` overrides, and leave global and project settings unchanged unless the user requests a change.

Auto mode remains required whether sandboxing is enabled or disabled. It controls action approvals independently of sandboxing.

## Data handling

- Claude Max is a consumer account. Fable 5.1 requires at least 30 days of server-side retention. Model Improvement can extend consumer-data retention.
- Persisted local transcripts are plaintext under `~/.claude/projects` and default to 30-day cleanup.
- Use `--no-session-persistence` when resumption is unnecessary.
- Use the Claude Agent SDK instead of shell parsing when building an application around the agent loop.
