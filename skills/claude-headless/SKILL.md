---
name: claude-headless
description: Use when you need Anthropic's Fable model and are working outside the Claude Code harness. Runs Claude Code headlessly
---

# Claude Code headless

Use `claude -p` for bounded delegated work. Default to `--model fable --effort medium`, unless the user specifies another model or effort. Check the installed `claude --version` and relevant `--help` flags; examples were checked against 2.1.278; the measurements behind them are in the agent-skills repo under `research/claude-codex-headless-empirical-2026-09-20.md`.

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
timeout -s INT -k 30 3600 claude -p \
  --model fable --effort medium \
  --permission-mode auto --permission-prompts none \
  --strict-mcp-config --no-session-persistence \
  --output-format json < prompt.txt >out.json 2>err.log
```

Run from the intended repository using the calling tool's working-directory parameter. Store prompts and logs in a private per-run directory. Pass the prompt as a file on stdin rather than interpolating it into shell code. Piped stdin is appended to any prompt argument and has a 10 MB limit; pass paths for larger inputs. Web search works in this mode without extra configuration.

## Time

A killed run loses its in-flight tool calls, so arrange for the run to end on its own.

- The `timeout` is a guard against a hung process, not a schedule. Measured on Opus at low effort: web lookup under 1 minute, bounded repository research 2–5 minutes, a task with subagents 10 minutes and more; Fable at medium or high takes longer. Set the ceiling at several times the expected duration; 3600 s is a sound default. Run anything beyond a few minutes in the background.
- Send SIGINT, not the default SIGTERM (`timeout -s INT -k 30`). SIGINT ends the turn and writes a `result` with `subtype: error_during_execution` and `terminal_reason: aborted_streaming`; SIGTERM exits 143 and records nothing.
- When a deadline exists, put it in the prompt rather than in the `timeout`. This wording made the agent finish early with a normal result and a list of gaps:

  > Time budget: 10 minutes of wall-clock time from your first action. Run `date +%s` first and again after every few tool calls. When 8 minutes have passed, stop exploring and deliver the report with what you have, listing the areas you did not reach under a heading "Not covered".

  The agent errs early (a 4-minute budget ended after 2 minutes), so state the budget you can really afford.
- For work expected to pass ten minutes, ask for a progress file: findings appended to an absolute path after each area, and the final report in a second file.
- Judge a hang by silence, not by elapsed time. A healthy stream pauses up to about 90 seconds. Ten minutes without growth in the events file is a hang: interrupt it and resume.
- If a persisted run dies anyway, resume it ([sessions and monitoring](references/sessions-and-monitoring.md)). The task text, every completed tool result, and the subagents' saved transcripts survive, even when the kill came seconds after launch.

## Report run status and task acceptance separately

1. **Run completed:** retain the actual process exit code and parse the terminal `result` object, which is the last `result` line after the process has exited. A stream can hold an earlier `result` with `subtype: success` whose text says subagents are still running; the main turn ended and the process is waiting on them, so that line is not completion. Require exit `0`, `is_error: false`, `subtype: success`, and `terminal_reason: completed`. Missing fields or a missing terminal record mean completion is unconfirmed. Inspect `permission_denials` for work left undone. A wrapper's successful exit or one successful tool call is not the agent's exit. The terminal record does not cover background work: print mode kills background tasks 600 s after the main agent ends its turn and still reports success. Launch any run that may spawn subagents with `CLAUDE_CODE_PRINT_BG_WAIT_CEILING_MS=0`, and grep stderr for `Background tasks still running`; a hit means killed subagents, so report the run as interrupted and resume it ([sessions and monitoring](references/sessions-and-monitoring.md)).
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
