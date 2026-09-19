---
name: claude-headless
description: Use when you need Anthropic's Fable model and are working outside the Claude Code harness. Runs Claude Code headlessly
---

# Claude Code headless

Use `claude -p` for bounded delegated work. Default to `--model fable --effort medium`, unless the user specifies another model or effort. Check the installed `claude --version` and relevant `--help` flags; examples were checked against 2.1.261.

## Choose reasoning effort

Use `medium` for most tasks, including design/UI changes, styling, routine implementation, focused fixes, research, and ordinary reviews. Reserve `high` for the hardest tasks: difficult architectural trade-offs, elusive concurrency bugs, or changes with complex interacting constraints that require deep reasoning. Task length or file count alone does not justify high.

Choose between medium and high for each task or follow-up based on its actual complexity. A routine design correction after a high-effort architecture task should use medium. Honor an explicit user choice. The examples below use the default medium; replace it with high only when the task warrants it.

## Choose the workflow

- **One-shot consultation or extraction:** JSON output, no session persistence. Provide the needed context and a concrete deliverable.
- **Implementation or follow-up review:** persisted session, stream JSON, one owner responsible for launch, monitoring, and resumption. Read [delegation and review](references/delegation-and-review.md) and [sessions and monitoring](references/sessions-and-monitoring.md) before launching.
- **MCP or structured output:** read [MCP and structured output](references/mcp-and-structured-output.md).
- **CLI failure, stalled output, uncertain process state, or exhausted quota:** read [problem reporting](references/problem-reporting.md) before retrying.

A one-shot request:

```bash
claude -p "<bounded task>" \
  --model fable --effort medium \
  --permission-mode auto --permission-prompts none \
  --strict-mcp-config --no-session-persistence \
  --output-format json >out.json 2>err.log
```

Run from the intended repository using the calling tool's working-directory parameter. Store prompts and logs in a private per-run directory. For longer prompts, use a file as stdin rather than interpolating its contents into shell code. Piped stdin is appended to the prompt and has a 10 MB limit; pass paths for larger inputs.

## Report run status and task acceptance separately

1. **Run completed:** retain the actual process exit code and parse the terminal `result` object. Require exit `0`, `is_error: false`, `subtype: success`, and `terminal_reason: completed`. Missing fields or a missing terminal record mean completion is unconfirmed. Inspect `permission_denials` for work left undone. A wrapper's successful exit or one successful tool call is not the agent's exit. The terminal record does not cover background work: print mode kills background tasks 600 s after the main agent ends its turn and still reports success. Launch any run that may spawn subagents with `CLAUDE_CODE_PRINT_BG_WAIT_CEILING_MS=0`, and grep stderr for `Background tasks still running`; a hit means killed subagents, so report the run as interrupted and resume it ([sessions and monitoring](references/sessions-and-monitoring.md)).
2. **Task accepted:** inspect the deliverable and verify the required behavior against the actual changed files. A clean Claude result is not a code review or proof of user-facing correctness.

A failed or interrupted run can leave useful, acceptable work. Report that run as failed/interrupted and accept its saved artifact only after independent verification. Resolve writer liveness before another agent edits the same files.

Read final text from `.result`, structured data from `.structured_output` when requested. `.session_id` is the conversation ID, not the calling tool's process/session handle. `.modelUsage` may contain Fable for the task and Haiku for Auto classification; that alone is not a model switch. Check the task model against the user's selection.

CLI startup errors may produce no JSON. In-run errors can still produce a result, even one with `subtype: success`. Keep stdout and stderr separate, and do not pipe away the process exit status before checking it.

## Permissions and authentication

Keep `--permission-mode auto --permission-prompts none`. Inherit existing sandbox settings; do not alter global or project configuration merely to get a run through. For read-only work, state that scope and restrict available tools when useful while retaining Auto.

Use `--strict-mcp-config` when no MCP server is needed. It excludes account and repository MCP configuration, not hooks, plugins, or all other customization. `--tools` selects available tools; `--allowedTools` pre-approves matching calls and is not an isolation boundary. Resolve blocked actions within existing controls; do not switch to permission bypass.

A directory never opened interactively, typically a fresh git worktree, is untrusted: stderr says `Ignoring N permissions.allow entries from .claude/settings.json: this workspace has not been trusted`, and the project allowlist is dropped (Auto still passes the calls, hooks still run). Open the directory interactively once, or set `projects[<path>].hasTrustDialogAccepted` in `~/.claude.json`.

Use the data access already authorized by the user. Ask only when a new boundary actually needs approval, not again for the same approved scope. Keep credentials out of prompts, result excerpts, and reports. Account privacy/retention terms depend on the service and settings; verify them when relevant rather than assuming all Claude launches use one account type.

For authentication problems, use `claude auth status`; for CLI health, `claude doctor`. Normal print mode supports Claude.ai login. `--bare` skips OAuth/keychain authentication and is not a drop-in optimization for a subscription session. Native installations may update independently; do not update the CLI during a task unless needed and authorized.
