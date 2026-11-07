---
name: codex-headless
description: Use when you need an OpenAI model and are working outside the Codex harness. Runs Codex CLI headlessly with the model and reasoning effort the task calls for.
---

# Codex headless (`codex exec`)

Checked against `codex-cli 0.166.0` on 2026-11-07 (help output, release notes 0.163.0–0.166.0, one smoke run). Evidence: `research/codex-headless-recheck-2026-11-07.md` and earlier notes in the agent-skills repo.

## Auto-review invariant

Every invocation, `resume` and background runs included, carries both settings on the command line, even when `~/.codex/config.toml` already sets them:

```bash
-c approval_policy=on-request -c approvals_reviewer=auto_review
```

This and the sandbox are the user's safety decisions: a blocked action is not retried with weaker approvals or sandbox, and `--dangerously-bypass-approvals-and-sandbox` and `--dangerously-bypass-hook-trust` stay unused.

## One-shot run

```bash
timeout -k 15 3600 codex exec --ephemeral --json --skip-git-repo-check \
  -m <model> -c model_reasoning_effort=<effort> \
  --sandbox workspace-write \
  -c approval_policy=on-request -c approvals_reviewer=auto_review \
  < prompt.txt >events.jsonl 2>progress.log
```

- With the prompt on stdin, stderr starts with `Reading prompt from stdin...`; that is normal.
- `--skip-git-repo-check` is needed only outside a Git repository.
- `--ephemeral` leaves no session to resume or inspect; omit it for anything longer than a quick lookup. Codex still writes client state under `CODEX_HOME`, so a parent sandbox that blocks those writes stops startup before the first JSONL event.
- Follow-up turns and long runs: [sessions and monitoring](references/sessions-and-monitoring.md). MCP servers: [MCP](references/mcp.md). What went wrong in the user's real runs, dated with the CLI version: [observed in use](references/observed-in-use.md).
- A CLI or harness failure (auth, unavailable model, transport, state or sandbox initialization, malformed output, stall) gets a report: [problem reporting](references/problem-reporting.md).

## Output and completion

- `--json` writes JSONL to stdout, progress to stderr. A good stream starts with `thread.started` (carrying `thread_id`) and ends with `turn.completed`; an error or failed event, a non-zero exit, or no terminal event means failure. Exit 0 alone does not prove completion. SIGINT and a `timeout` kill leave no terminal event.
- Final answer:

  ```bash
  jq -rs '[.[] | select(.type=="item.completed" and .item.type=="agent_message")] | last | .item.text' events.jsonl
  ```

- JSONL does not name the model that ran. The rollout of a non-ephemeral run does, one record per turn:

  ```bash
  zstdcat -f ~/.codex/sessions/*/*/*/rollout-*-<thread_id>.jsonl* \
    | jq -c 'select(.type=="turn_context") | .payload | {model, effort, sandbox: .sandbox_policy.type, approval_policy, approvals_reviewer, cwd}'
  ```

  Rollouts older than about a week are stored compressed as `.jsonl.zst` (seen 2026-11-07); `zstdcat -f` reads both forms.
- `-o file` writes the final message and still prints it to stdout.

## Model and effort

The user names model and effort in the chat; otherwise `~/.codex/config.toml` holds the default (the built-in default since 0.161.0 is `gpt-6.1-sol`). Pass `-m` and `-c model_reasoning_effort=` on every invocation, resumes included. `codex debug models | jq -r '.models[].slug'` lists what the installed CLI offers; a model the account cannot use fails at the first request.

## Sandbox

- `--sandbox workspace-write` allows writes in the working root and `/tmp`. `--sandbox read-only` suits untrusted input and audits, but then the agent cannot write a progress or report file.
- Auto-review judges requests beyond the sandbox; it does not widen the sandbox or turn on network. Network intent is set with `-c sandbox_workspace_write.network_access=true|false`. The built-in web search works in every sandbox mode, `read-only` included.
- `--approve-for-me` is shorthand for a new workspace-write run with Auto-review; `exec resume` does not accept it, so scripts use the explicit `-c` form.

## Kills and subagents

A killed run loses its in-flight commands and all subagent work, so the `timeout` is a guard against a hang, set well above the expected duration (3600 s is a sound default), not a schedule. A deadline goes in the prompt instead. `codex exec` waits for its subagents; after a kill a resumed agent has claimed it could still reach them when it could not (0.155.1), so a run that may be cut short is better told to work without subagents.

## Clean runs

Every run sees the user's skills (`~/.agents/skills`, `CODEX_HOME/skills`) and the project's `AGENTS.md`. For a test agent that must not, a per-run `CODEX_HOME` with `project_doc_max_bytes = 0`, memories off, and a `[[skills.config]] path = "...", enabled = false` entry per `SKILL.md` gave a prompt with no skill listing and no AGENTS.md (0.159.2, 2026-09-30). `--ignore-user-config` skips `config.toml` but keeps auth; whether it also hides skills was not tested.
