# Sessions and monitoring

Checked against Claude Code 2.1.316 and https://code.claude.com/docs/en/headless on 2026-11-07.

## Launch with a record

For coding work use a persisted conversation and `--output-format stream-json --verbose`, in a private per-run directory. `--session-id <uuid>` fixes the conversation ID before the run starts, so a kill seconds after launch cannot lose it, and `--resume <id>` finds it from any directory. The conversation ID, the Claude PID and the calling tool's process handle are three different things; record all three, plus the working directory, model and effort.

```bash
# In the intended repository, with a private run_dir holding prompt.txt.
session_id=$(cat /proc/sys/kernel/random/uuid); printf '%s\n' "$session_id" > "$run_dir/session-id"
CLAUDE_CODE_PRINT_BG_WAIT_CEILING_MS=0 timeout -s INT -k 30 3600 claude -p \
  --model <model> --effort <effort> --session-id "$session_id" \
  --permission-mode auto --permission-prompts none \
  --strict-mcp-config --output-format stream-json --verbose \
  < "$run_dir/prompt.txt" > "$run_dir/events.jsonl" 2> "$run_dir/stderr.log" &
claude_pid=$!; printf '%s\n' "$claude_pid" > "$run_dir/pid"
if wait "$claude_pid"; then run_exit=0; else run_exit=$?; fi
printf '%s\n' "$run_exit" > "$run_dir/exit-code"
```

A bare trailing `&` with no wait and exit record loses the exit status; a missing exit file means unknown, not success, and a reused run directory can describe an earlier process.

## Background work at the end of a run

After the main agent ends its turn, a `-p` run stays open for background work. The docs say: a background command the main conversation started keeps the run open until it exits or hits its own time limit (default 30 min); background subagents and workflows keep it open until they complete; everything else is stopped after `CLAUDE_CODE_PRINT_BG_WAIT_CEILING_MS` of idle waiting (default 10 minutes), dropping its partial result. On 2.1.316 a busy background subagent was stopped at the ceiling anyway, with a clean result ([observed in use](observed-in-use.md)). So set the variable to `0` for any run that may background work, and before reporting completion check stderr for `Background tasks still running` (the line names what was stopped) and `subagent_stats` in the final result. `--max-budget-usd` also stops remaining background work when reached.

`CLAUDE_CODE_DISABLE_BACKGROUND_TASKS=1` (process-local) turns off background Bash, background subagents and automatic backgrounding, but not shell `&`. `--bare` does the same and more, but also drops subscription login (SKILL.md).

Subagents run at the session's effort unless the Agent call names one or `CLAUDE_CODE_SUBAGENT_EFFORT` is set (2.1.316). Since 2.1.302 a subagent's own Agent calls run in the foreground by default in `-p`, and fork subagents are on (`CLAUDE_CODE_FORK_SUBAGENT=0` turns them off).

`--max-turns` and `--max-budget-usd` are not wall-clock deadlines. The outer execution tool's yield interval is not necessarily a kill timeout; track the outer deadline separately from Claude's own Bash-tool timeouts.

## Monitor

Read new events since the previous offset. Events carry `timestamp`. Subagent messages carry `parent_tool_use_id`; `system/task_started`, `task_progress`, `task_notification` and `background_tasks_changed` track background work, `--forward-subagent-text` adds subagent text, `system/api_retry` marks retries and `permission_denied` marks denials. Tool activity, file changes or a quiet log inform diagnosis but do not establish that the process ended.

Before declaring a run dead, establish what process namespace the observation sees: in Codex a sandbox `/proc` may omit host processes. Prefer the original execution handle and its wait result; otherwise correlate a permitted host observation with the recorded PID, its start time and parentage. A log descriptor can be inherited by a non-Claude child. Session files and log timestamps are context, not proof of liveness or death.

## Resume only after the old process has exited

Resume by the recorded ID with `--resume`; `--continue` takes the latest conversation in the working directory and can pick another task, and `--no-session-persistence` makes resuming impossible. Keep the model, effort and permission flags unless the user changes them, and write each turn to new output files.

On resume the interrupted turn is left as it is and the new prompt drives the conversation (`CLAUDE_CODE_RESUME_INTERRUPTED_TURN=1` continues the turn instead). So after a kill, resume with the ceiling lifted and a short message saying what happened: the process ended, why, which subagents were stopped. The agent keeps its context and can re-reach saved subagents with SendMessage; the task need not be restated.

```bash
CLAUDE_CODE_PRINT_BG_WAIT_CEILING_MS=0 claude -p --resume "$session_id" ...
```

One writer per overlapping file set, and one orchestrator launches and resumes each run; an observer may read logs but does not launch a replacement. A second writer started because monitoring is uncertain races the first. To stop a writer, identify that exact process (no process-name kills), stop it, wait for its exit and its test children. A correction brief goes to the same persisted session after its process has exited. A message written to an inbox file reaches the agent only if its prompt said when to read it.

Native `claude --bg` sessions (`claude agents`, `attach`, `logs`, `stop`) are a separate lifecycle; `-p` rejects `--bg`, and switching lifecycles is not a recovery step.
