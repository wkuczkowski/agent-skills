---
name: codex-headless
description: Run OpenAI Codex CLI programmatically via codex exec. Use when delegating a task to a Codex agent, scripting codex in shell or CI, or when the user mentions running Codex headless or programmatically.
---

# Codex headless (`codex exec`)

Workflow for driving OpenAI Codex CLI non-interactively. Everything below was verified by actually running it (codex-cli 0.145.0).

## Core call

```bash
codex exec -m gpt-5.6-sol -c model_reasoning_effort=medium \
  --sandbox workspace-write \
  -c approval_policy=on-request -c approvals_reviewer=auto_review \
  "<task>" 2>progress.log
```

- **stdout carries only the agent's final message** (pipe-friendly); progress goes to stderr.
- Prompt as argument, or from stdin: `cat prompt.txt | codex exec -`. Piping data *and* giving a prompt argument appends stdin as a `<stdin>` block.
- Exit code: `0` success, non-zero on error.
- Outside a git repo add `--skip-git-repo-check`; `-C <dir>` sets the working root.

## Model and reasoning effort

Always use the newest flagship **sol** variant of the GPT family (currently `gpt-5.6-sol`; when a newer `gpt-*-sol` ships, use that). Pick effort by task complexity via `-c model_reasoning_effort=...`:

- **low** — mechanical work: simple repo questions, formatting, renames, one obvious edit (answers in seconds)
- **medium** — default: implement a feature, locate-and-fix a bug, multi-file refactor
- **high** — hard reasoning: gnarly debugging, architecture, security review, multi-step tasks with traps

(`minimal` and `xhigh` also exist; reach for `xhigh` only when `high` proved insufficient.)

## Sandbox and approvals

- **Default: `--sandbox workspace-write`** — the agent can create and edit files in the working root and `/tmp`; the rest of the filesystem stays protected. A write-capable agent is far more useful than a read-only one.
- **Approvals: "Approve for me"** — `-c approval_policy=on-request -c approvals_reviewer=auto_review`. Escalations beyond the sandbox (writes outside writable roots, blocked network) go to a separate reviewer agent instead of a human. Verified in headless: with `auto_review` a justified out-of-sandbox write was approved and executed; with `approvals_reviewer=user` the same escalation fails with "approval escalation is disabled". (Docs describe auto-review as interactive-only — empirically it works in `exec`.) The locked-down alternative for CI is `-c approval_policy=never`: escalations then fail immediately and the failure is returned to the model.
- Network inside workspace-write is **off by default**; enable deliberately: `-c sandbox_workspace_write.network_access=true`.
- Restrict to `--sandbox read-only` only in the rare cases that demand it: untrusted inputs, pure analysis, audits.
- `--dangerously-bypass-approvals-and-sandbox` (`--yolo`) only inside an already-isolated container/VM. Deprecated, avoid: `--full-auto`, `-a on-failure`.
- `codex exec` has no `--ask-for-approval` flag — set approvals via `-c approval_policy=...` or `~/.codex/config.toml`.

## Sessions

```bash
codex exec resume --last "follow-up question"     # most recent session in this directory
codex exec resume <SESSION_ID> "follow-up"        # specific session
```

Resumed sessions keep full context. Get the session id from the `--json` stream (`thread.started` → `thread_id`).

## Monitoring a background run

For long tasks, launch in the background with `--json` streaming to a file, then poll the file:

```bash
codex exec --json -m gpt-5.6-sol --sandbox workspace-write "<long task>" > events.jsonl 2>/dev/null &
```

- **What is it doing right now** — the most recent items; an `item.started` without a matching `item.completed` is the action currently in flight (for `command_execution` you see the exact command):
  ```bash
  jq -r 'select(.type|startswith("item"))
         | "\(.type) \(.item.type) \(.item.command // .item.text // "" | tostring | .[0:80])"' events.jsonl | tail -3
  ```
- **Is it stuck** — check the file's mtime: if `events.jsonl` stops growing for minutes, the agent is stalled at its last `item.started`.
- **Is it done** — the stream ends with a `turn.completed` event (includes token usage); the last `agent_message` item is the final answer.
- **Follow up** — interrogate or continue the same agent with `codex exec resume <thread_id>` (`thread_id` comes from the `thread.started` event).

Verify deliverables yourself (the file exists, tests pass) rather than trusting the final message alone.

## Structured and machine-readable output

- `--output-schema schema.json` — final answer is **pure JSON on stdout** conforming to the schema (no unwrapping needed).
- `--json` — JSONL event stream: `thread.started` (thread_id), `item.started`/`item.completed` (commands, messages), `turn.completed`. Final answer:

  ```bash
  codex exec --json "..." | jq -rs '[.[] | select(.type=="item.completed" and .item.type=="agent_message")] | last | .item.text'
  ```

- `-o file.md` / `--output-last-message file.md` — write the final message to a file.

## Config defaults

`~/.codex/config.toml` holds the defaults (`model`, `model_reasoning_effort`, `approval_policy`, `approvals_reviewer`, `sandbox_mode`); any `-c key=value` overrides one run. `--ephemeral` skips persisting session files.
