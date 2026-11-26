# Sessions and monitoring

Read this when a Codex task needs follow-up turns or runs long enough to watch.

## Resume

```bash
codex exec resume -m <model> -c model_reasoning_effort=<effort> \
  -c sandbox_mode=<mode> \
  -c approval_policy=on-request -c approvals_reviewer=auto_review \
  <THREAD_ID> "follow-up"
```

- The id is the `thread_id` of the first stdout event, `thread.started`; it is there even in a run killed seconds after launch. The task text and every completed command's output survive the kill.
- `resume` still accepts neither `--sandbox` nor `-C` (help, `0.172.0`), but since `0.172.0` (`#62194`, verified live 2026-11-26) it restores the saved thread's original working directory, workspace roots, and sandbox permissions rather than taking the caller's current directory or falling back to `config.toml`, while respecting explicit `-c` overrides (such as `-c sandbox_mode=` or `-c model_reasoning_effort=`). Passing `-c sandbox_mode=` explicitly remains safe when you want to pin or change the mode. The rollout's `turn_context` records (SKILL.md, Output and completion) show what a resumed turn actually ran with.
- `--last` picks the newest session in the current directory (`--all` disables that filter); under parallel runs that can be someone else's, so resume by id.

## Watching a background run

- Events carry no timestamps and are sparse: a command appears when it starts and when it completes. The file's mtime is the only sign of a stall. An `item.started` without its `item.completed` is the current action.
- Subagent activity showed only as `collab_tool_call` items with `tool: "wait"`; spawns and subagent output were not itemised (0.155.1; 0.163–0.166 reworked multi-agent internals, not re-checked).
- Completion is a terminal `turn.completed`; the last completed `agent_message` is the answer.
- For work past ten minutes, a progress file the agent appends to after each area, at an absolute path inside the writable roots, keeps partial results if the run dies.
