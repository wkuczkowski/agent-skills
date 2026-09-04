---
name: codex-headless
description: Run OpenAI Codex CLI programmatically via codex exec. Use when delegating a task to a Codex agent, scripting codex in shell or CI, or when the user mentions running Codex headless or programmatically.
---

# Codex headless (`codex exec`)

Drive OpenAI Codex CLI non-interactively. The commands below were verified with `codex-cli 0.153.2`.

## Auto-review invariant

Every invocation must use Auto-review, including `resume`, background runs, read-only audits, and CI:

```bash
-c approval_policy=on-request -c approvals_reviewer=auto_review
```

Keep both settings explicit on the command line so user, project, or system config cannot select a different reviewer. For a new `codex exec` run, `--approve-for-me` is a shorter equivalent that also selects the `workspace-write` sandbox. Prefer the explicit settings in scripts because the same form works with `codex exec resume`.

## Core call

```bash
codex exec -m gpt-5.6-sol -c model_reasoning_effort=medium \
  --sandbox workspace-write \
  -c approval_policy=on-request -c approvals_reviewer=auto_review \
  "<task>" 2>progress.log
```

- **stdout carries only the agent's final message** (pipe-friendly); progress goes to stderr.
- Pass the prompt as an argument or through stdin: `cat prompt.txt | codex exec -c approval_policy=on-request -c approvals_reviewer=auto_review -`. Piping data *and* giving a prompt argument appends stdin as a `<stdin>` block.
- Exit code: `0` success, non-zero on error.
- Outside a git repo add `--skip-git-repo-check`; `-C <dir>` sets the working root.

## Model and reasoning effort

Always use the newest flagship **sol** variant of the GPT family (currently `gpt-5.6-sol`; when a newer `gpt-*-sol` ships, use that). Pick effort by task complexity via `-c model_reasoning_effort=...`:

- **low** for mechanical work such as simple repo questions, formatting, renames, or one obvious edit
- **medium** by default for feature work, bug fixes, and multi-file refactors
- **high** for hard debugging, architecture, security reviews, or multi-step tasks with traps

(`minimal` and `xhigh` also exist; reach for `xhigh` only when `high` proved insufficient.)

## Sandbox and approvals

- **Default: `--sandbox workspace-write`.** The agent can create and edit files in the working root and `/tmp`; the rest of the filesystem stays protected.
- **Auto-review.** Escalations beyond the sandbox, such as writes outside writable roots or blocked network calls, go to a separate reviewer agent. Auto-review changes who reviews a request. It does not widen the sandbox or enable network access.
- Network inside workspace-write is **off by default**, but loaded config can override it. Set the intended value explicitly when it matters: `-c sandbox_workspace_write.network_access=false` or `true`.
- Restrict to `--sandbox read-only` only in the rare cases that demand it: untrusted inputs, pure analysis, audits.
- Use `--dangerously-bypass-approvals-and-sandbox` only inside an already-isolated container or VM. It bypasses Auto-review, so it is outside this skill's normal workflow.
- `--full-auto` and `-a on-failure` are deprecated.
- `codex exec` has no `--ask-for-approval` flag. Set approvals via `-c approval_policy=...` or `~/.codex/config.toml`.

## Sessions

```bash
codex exec resume \
  -c approval_policy=on-request -c approvals_reviewer=auto_review \
  --last "follow-up question"

codex exec resume \
  -c approval_policy=on-request -c approvals_reviewer=auto_review \
  <SESSION_ID> "follow-up"
```

Resumed sessions keep full context. Get the session id from the `--json` stream (`thread.started` → `thread_id`).

## Monitoring a background run

For long tasks, launch in the background with `--json` streaming to a file, then poll the file:

```bash
codex exec --json -m gpt-5.6-sol --sandbox workspace-write \
  -c approval_policy=on-request -c approvals_reviewer=auto_review \
  "<long task>" > events.jsonl 2>progress.log &
```

- **What is it doing right now?** Inspect the most recent items. An `item.started` without a matching `item.completed` is the action currently in flight. For `command_execution`, the event includes the exact command.
  ```bash
  jq -r 'select(.type|startswith("item"))
         | "\(.type) \(.item.type) \(.item.command // .item.text // "" | tostring | .[0:80])"' events.jsonl | tail -3
  ```
- **Is it stuck?** Check the file's mtime. If `events.jsonl` stops growing for minutes, the agent is stalled at its last `item.started`.
- **Is it done?** The stream ends with a `turn.completed` event that includes token usage. The last `agent_message` item is the final answer.
- **Follow up.** Continue the same agent with `codex exec resume -c approval_policy=on-request -c approvals_reviewer=auto_review <thread_id> "<follow-up>"`. The `thread_id` comes from the `thread.started` event.

## MCP servers

Verified end-to-end: `codex exec` loads MCP servers from config and calls their tools headless with no approval friction when the server sets `default_tools_approval_mode = "approve"`.

```toml
[mcp_servers.my-server]
command = "uv"
required = true
default_tools_approval_mode = "approve"
args = ["--directory", "/abs/path/to/project", "run", "python", "-m", "my_server.main"]
```

- The config lives in `~/.codex/config.toml` globally or in a project-level `.codex/config.toml` within a trusted project root (`trust_level = "trusted"` under `[projects."<path>"]` in the global config).
- Set `required = true` when the run cannot succeed without that server. Codex then fails at startup instead of silently continuing without it.
- With `default_tools_approval_mode = "approve"` the agent listed and called the server's tools under `--sandbox read-only` without any prompt or escalation.
- Prefer absolute paths in `args`; relative paths resolve against the repo you launch from.

## Structured and machine-readable output

- `--output-schema schema.json` makes the final answer pure JSON on stdout, conforming to the schema with no unwrapping needed.
- `--json` produces a JSONL event stream with `thread.started`, `item.started`, `item.completed`, and `turn.completed`. Extract the final answer with:

  ```bash
  codex exec --json \
    -c approval_policy=on-request -c approvals_reviewer=auto_review \
    "..." | jq -rs '[.[] | select(.type=="item.completed" and .item.type=="agent_message")] | last | .item.text'
  ```

- `-o file.md` / `--output-last-message file.md` writes the final message to a file and still prints it to stdout.

## Config defaults

`~/.codex/config.toml` holds the defaults (`model`, `model_reasoning_effort`, `approval_policy`, `approvals_reviewer`, `sandbox_mode`); any `-c key=value` overrides one run. This skill still sets Auto-review explicitly on every invocation.

`--ephemeral` skips persisting session rollout files. Codex can still need write access to initialize its local app-server state. If startup fails with a read-only filesystem error before the first JSONL event, allow the outer process to write to its `CODEX_HOME`; keep the invoked agent's own sandbox at the requested level.
