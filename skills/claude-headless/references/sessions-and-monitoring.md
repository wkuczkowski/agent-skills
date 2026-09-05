# Sessions and monitoring

## Launch with an owner and a record

For coding work, use a persisted conversation and `--output-format stream-json --verbose`. Use a unique private run directory for the prompt, events, stderr, and exit status. Record the working directory, selected model/effort, launch time, calling tool's handle, Claude PID/start identity when available, and Claude conversation ID from `system/init` or the final result. These identifiers are different.

Keep the calling tool attached to a supervised process and retain its handle when it yields. A bare trailing `&` without a wait/exit record loses useful supervision. If a shell wrapper is needed, have it record the child PID and wait for that child before writing its exit status. A missing exit file means unknown, not success. Confirm the record belongs to the current run; reused log or exit files can describe an earlier process.

```bash
# Run in the intended repository. Prepare a private run_dir and prompt.txt first.
claude -p \
  --model fable --effort medium \
  --permission-mode auto --permission-prompts none \
  --strict-mcp-config --output-format stream-json --verbose \
  < "$run_dir/prompt.txt" > "$run_dir/events.jsonl" 2> "$run_dir/stderr.log" &
claude_pid=$!
printf '%s\n' "$claude_pid" > "$run_dir/pid"
if wait "$claude_pid"; then run_exit=0; else run_exit=$?; fi
printf '%s\n' "$run_exit" > "$run_dir/exit-code"
exit "$run_exit"
```

The outer execution tool must allow the intended task duration. Its yield interval is not necessarily a kill timeout. Track outer task deadlines separately from Claude Bash-tool timeouts. `--max-turns` and `--max-budget-usd` do not establish a wall-clock deadline; check supported flags in the installed CLI before using them.

For a supervised task that does not need Claude-managed background work, consider process-local `CLAUDE_CODE_DISABLE_BACKGROUND_TASKS=1`. It disables background Bash/subagent tools and automatic backgrounding, but not shell `&`. It is an optional simplification, not a proven fix for unexpected SIGTERM. Leave global settings unchanged. [Official variable reference](https://code.claude.com/docs/en/env-vars).

## Monitor without confusing activity and completion

Read new events since the previous offset, not the entire transcript repeatedly. Summarize the latest completed step, current tool, and blocker. Tool activity, file changes, or a quiet log can inform diagnosis but cannot establish process termination. Check run status from SKILL.md when the terminal result and actual exit arrive; assess the saved artifact separately.

Before declaring a run dead, establish what process namespace the observation sees. In Codex, a sandbox `/proc` may omit host processes. Prefer the original execution handle and its wait result; otherwise correlate a permitted host observation with the recorded PID/start identity, exact log descriptors, or supervisor record. A log descriptor can be inherited by a non-Claude child; confirm executable, start identity, and parentage before treating it as the writer. Session files and log timestamps provide context, not independent proof of liveness or death. Request the specific missing access if required, rather than treating an empty listing as proof.

## Resume only after termination is confirmed

Use an explicit recorded Claude session ID with `--resume`. `--continue` selects the latest conversation in the working directory and can select another task. Retain the selected model and permission flags unless the user changes them; choose medium or high for the follow-up using the complexity rule in SKILL.md; use new output files for each turn. `--no-session-persistence` prevents resumption. If persistence was disabled or no usable session exists, start a fresh conversation only after confirming exit, with a checkpoint of verified files and remaining work.

Never start a second writer merely because monitoring is uncertain. Confirm the old writer has exited; if authorized to stop it, identify that exact process, stop it, wait for exit, and check its owned test children before resuming. Avoid broad process-name kills. If several orchestrators can launch work, use a shared ownership record or lock.

Interrupted runs can leave useful edits. Inspect those files and recorded checks, then resume with a bounded remaining task. A frozen snapshot helps review an interrupted candidate without racing a live writer.

Native `claude --bg` is a separate lifecycle from `claude -p`. Its `agents/logs/attach/stop` handles are not print-mode process handles. Do not switch lifecycle modes as an implicit recovery step. [Official headless lifecycle](https://code.claude.com/docs/en/headless).
