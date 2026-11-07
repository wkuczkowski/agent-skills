---
name: claude-headless
description: Use when you need an Anthropic model and are working outside the Claude Code harness. Runs Claude Code headlessly with the model and reasoning effort the task calls for.
---

# Claude Code headless

`claude -p` runs bounded delegated work. The user names the model and effort in the chat; pass both explicitly on every run (`--model`, `--effort`), including resumes. Checked against Claude Code 2.1.316, its `--help` and the changelog on 2026-11-07 (`research/claude-headless-recheck-2026-11-07.md`); if the installed `claude --version` is newer, check `--help` for the flags used here. Dated observations from real runs, with their CLI versions, are in [observed in use](references/observed-in-use.md).

- Implementation, follow-up review, or anything that may run subagents: persisted session and stream JSON; read [sessions and monitoring](references/sessions-and-monitoring.md) before launching.
- MCP or schema-validated output: [MCP and structured output](references/mcp-and-structured-output.md).
- CLI failure, exhausted quota, or uncertain process state: [problem reporting](references/problem-reporting.md) before retrying.

A one-shot request:

```bash
timeout -s INT -k 30 3600 claude -p \
  --model <model> --effort <effort> \
  --permission-mode auto --permission-prompts none \
  --strict-mcp-config --no-session-persistence \
  --output-format json < prompt.txt >out.json 2>err.log
```

Pass the prompt as a file on stdin rather than interpolating it into shell code. Piped stdin is appended to any prompt argument and capped at 10 MB (larger input exits non-zero); pass paths for larger inputs.

## Time and signals

A killed run loses its in-flight tool calls, so the run should end on its own: a deadline belongs in the prompt, and the `timeout` is only a guard against a hung process, set at several times the expected duration (3600 s is a sound default).

Send SIGINT, not the default SIGTERM (`timeout -s INT -k 30`). SIGINT ends the turn and writes a `result` with `subtype: error_during_execution` and `terminal_reason: aborted_streaming`; SIGTERM exits 143 and records no result for the turn in progress.

Judge a hang by silence in the events file, not by elapsed time: healthy streams paused up to about 90 s (2.1.278), and ten minutes without growth is a hang. A persisted run that dies keeps the task text, every completed tool result and the subagents' saved transcripts, so resume it.

## Run status and task acceptance are separate

1. **Run completed:** keep the real process exit code and parse the terminal `result`, the last `result` line after the process has exited. An earlier `result` with `subtype: success` can say subagents are still running; the main turn ended and the process was waiting on them. Require exit `0`, `is_error: false`, `subtype: success` and `terminal_reason: completed`; missing fields or no terminal record mean completion is unconfirmed. Check `permission_denials` for work left undone. The terminal record does not cover background work: on 2.1.316 a background subagent stopped at the print-mode ceiling still left exit 0 and a clean result, visible only in stderr (`Background tasks still running`) and in `subagent_stats` (`completed` below `spawned`, or `killed.*` above zero). Launch any run that may spawn subagents with `CLAUDE_CODE_PRINT_BG_WAIT_CEILING_MS=0`; if work was stopped, report the run as interrupted and resume it.
2. **Task accepted:** inspect the deliverable and check the required behaviour against the files actually changed. A clean result is not a code review.

A failed or interrupted run can leave acceptable work: report the run as failed or interrupted, accept the artifact only after independent checks, and settle whether the old writer is still alive before anyone else edits the same files.

Final text is `.result`, schema output `.structured_output`. `.session_id` is the conversation ID, not the calling tool's process handle. `.modelUsage` can list a Haiku model next to the task model when auto mode's classifier ran; that alone is not a model switch, but check the task model against the user's choice.

Keep stdout and stderr separate and do not pipe away the exit status. A startup failure prints the error and exits 1 with no JSON (before 2.1.314 it could exit 0 with no output); a failure inside the run, such as missing authentication, is printed as the result.

## Permissions and authentication

Keep `--permission-mode auto --permission-prompts none` and never switch to permission bypass (`--dangerously-skip-permissions`, `bypassPermissions`); these are the user's safety decisions. With `--permission-prompts none` anything that would prompt is denied, Claude is told not to retry it, and `AskUserQuestion` is removed. Since 2.1.302 a `-p` run with no permission mode set can start in Manual mode, where calls needing approval are denied, so pass the mode on every run. Inherit existing sandbox settings and do not change global or project configuration to get a run through; resolve a denied action within existing controls. For read-only work, say so in the prompt and narrow `--tools` if useful, keeping Auto.

`--strict-mcp-config` excludes account and repository MCP servers, not hooks, plugins or other customization. `--tools` selects the available tools; `--allowedTools` pre-approves matching calls and is not an isolation boundary (in auto mode a bare `Bash` entry is dropped and each command is still classified).

`-p` skips the workspace trust dialog. In a directory never opened interactively, typically a fresh git worktree, stderr says `Ignoring N permissions.allow entries from .claude/settings.json: this workspace has not been trusted` and the project allowlist is dropped, while the project's hooks and `.mcp.json` servers still run. The fix the CLI names (open the directory interactively once, or `projects[<path>].hasTrustDialogAccepted` in `~/.claude.json`) is a configuration change for the user to make.

Credentials stay out of prompts, result excerpts and reports. Normal `-p` uses the Claude.ai login (`claude auth status`; `claude doctor` for CLI health). `--bare` never reads OAuth or the keychain, so it is not a speed-up for a subscription session. The native install updates itself; do not update the CLI during a task unless the user asks.
