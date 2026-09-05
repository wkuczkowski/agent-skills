---
name: codex-headless
description: Use when you need OpenAI's Astra model and are working outside the Codex harness. Runs Codex CLI headlessly with Low or High reasoning effort chosen for the task.
---

# Codex headless (`codex exec`)

Drive OpenAI Codex CLI non-interactively. These instructions target `codex-cli 0.153.2`.

## Auto-review invariant

Every invocation must use Auto-review, including `resume`, background runs, read-only audits, and CI:

```bash
-c approval_policy=on-request -c approvals_reviewer=auto_review
```

Keep both settings explicit on the command line. Do not retry a blocked action by weakening approvals or the sandbox.

## Choose a run

Use this for a one-shot task:

```bash
codex exec --ephemeral --json --skip-git-repo-check \
  -m gpt-6-astra -c model_reasoning_effort=low \
  --sandbox workspace-write \
  -c approval_policy=on-request -c approvals_reviewer=auto_review \
  "<task>" >events.jsonl 2>progress.log
```

- Remove `--skip-git-repo-check` inside a Git repository.
- Omit `--ephemeral` when the task may need `resume`.
- Use `-C <dir>` to set the working root.
- For resumable or long-running work, read [sessions and monitoring](references/sessions-and-monitoring.md).
- For MCP, read [MCP servers](references/mcp.md) before constructing the command.

`codex exec` still writes client state under `CODEX_HOME` when `--ephemeral` is set. If a parent sandbox blocks startup before the first JSONL event, allow the outer process to write there. Keep the invoked Codex process on its requested sandbox and Auto-review settings.

If the CLI itself fails, hangs, selects an unavailable model, emits malformed output, or cannot initialize its state or sandbox, read [problem reporting](references/problem-reporting.md). Record the sanitized failure, tell the user where the report is, and continue with unaffected work.

## Output and completion

- `--json` writes JSONL to stdout and progress to stderr.
- A successful stream starts with `thread.started` and ends with `turn.completed`. Treat error or failed events, a non-zero exit, or a missing terminal event as failure.
- Extract the last completed agent message:

  ```bash
  jq -rs '[.[] | select(.type=="item.completed" and .item.type=="agent_message")] | last | .item.text' events.jsonl
  ```

- JSONL does not expose the resolved model. The explicit `-m` argument and captured command are the model record.
- Without `--json`, stdout contains the final message and progress goes to stderr.
- Pass the prompt as an argument or through stdin. Piping data while also passing a prompt appends stdin as a `<stdin>` block.
- Exit code `0` means the CLI completed. Still apply the JSONL checks above when machine-readable completion matters.
- `-o file.md` writes the final message to a file and still prints it to stdout.
- `--output-schema schema.json` makes the final answer conform to the supplied JSON schema.

## Model and effort

Use OpenAI Astra (`gpt-6-astra`) with either `low` or `high` reasoning effort. Choose by the depth of reasoning required, ambiguity, and interacting constraints, rather than task length or file count.

- **Astra Low:** well-defined tasks with a clear approach, such as routine edits, straightforward fixes, focused lookups, and implementation from an established plan.
- **Astra High:** tasks requiring substantial judgment or reasoning across several steps, such as ambiguous requirements, architecture tradeoffs, difficult debugging, security analysis, and changes with complex interactions.

Pass `-m gpt-6-astra` and `-c model_reasoning_effort=low` or `high` explicitly on every invocation, including resumed sessions. Reassess effort when the task changes; choose `high` when unresolved uncertainty could materially affect correctness. Use another model or effort level only when the user requests it.

## Sandbox and approvals

- Default to `--sandbox workspace-write`. It permits changes in the working root and `/tmp` while protecting the rest of the filesystem.
- Auto-review evaluates requests beyond that sandbox. It does not widen the sandbox or enable network access.
- Set network intent explicitly when it matters with `-c sandbox_workspace_write.network_access=false` or `true`.
- Use `--sandbox read-only` for untrusted input, audits, and analysis that must not change files.
- `--approve-for-me` is a shorter equivalent for new workspace-write runs. Prefer explicit settings in scripts because the same form works with `resume`.
- `--full-auto` and `-a on-failure` are deprecated.
- `--dangerously-bypass-approvals-and-sandbox` is outside this skill because it disables the required controls.

## Config defaults

`~/.codex/config.toml` can hold `model`, `model_reasoning_effort`, `approval_policy`, `approvals_reviewer`, and `sandbox_mode`. Command-line `-c` values override it. Keep Auto-review explicit on every invocation even when the config already sets it.
