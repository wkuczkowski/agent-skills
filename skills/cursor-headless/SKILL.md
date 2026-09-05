---
name: cursor-headless
description: Use when you need the Grok model available through Cursor and are working outside the Cursor harness. Runs Cursor Agent headlessly with Grok 4.6 High Fast.
---

# Cursor headless (`cursor-agent -p`)

Drive Cursor Agent CLI non-interactively. These instructions target `cursor-agent 2026.09.02-c22c1a3`.

## Choose a run

Use this for a one-shot task:

```bash
cursor-agent -p --trust --auto-review --sandbox disabled \
  --model cursor-grok-4.6-high-fast \
  "<task>" --output-format stream-json >events.jsonl 2>err.log
```

- `--trust` is mandatory in headless mode.
- Always pass `--auto-review --sandbox disabled` and the model explicitly.
- Use `--workspace <path>` to set the working root.
- For resumable, long-running, or multitask work, read [sessions, multitasking, and monitoring](references/sessions-multitasking-monitoring.md).
- For MCP, read [MCP servers](references/mcp.md) before constructing the command.

Cursor writes client state under `~/.cursor/projects`. If a parent sandbox blocks startup, allow the outer process to write there. Keep Cursor's own Auto Review setting and task scope unchanged.

If the CLI itself fails, hangs, selects the wrong model, emits malformed output, or cannot initialize state or permissions, read [problem reporting](references/problem-reporting.md). Record the sanitized failure, tell the user where the report is, and continue with unaffected work.

## Output and completion

- Prefer `stream-json` for automation. A successful stream starts with `system/init` and ends with a `result` event whose `.subtype` is `success` and `.is_error` is `false`.
- Confirm `system/init.model` is `Cursor Grok 4.6 High Fast`.
- With `--output-format json`, `.result` can concatenate text from several assistant turns. Do not treat it as an exact final-message field.
- Extract the last assistant text from a completed stream when exact output matters:

  ```bash
  jq -rs '[.[] | select(.type=="assistant") | .message.content[]?
           | select(.type=="text") | .text] | last' events.jsonl
  ```

- `system/init.permissionMode` may report `default` even when the command explicitly passes `--auto-review`. The explicit flag and captured command are the review-mode record.
- Keep stderr separate from stdout. A non-zero exit or an error result means failure.
- Piped stdin is appended to the prompt argument.
- Cursor has no structured-output schema flag. Ask for JSON when needed, but validate it in the caller.

## Model

Always use `cursor-grok-4.6-high-fast`. The slug selects Cursor Grok 4.6 with high reasoning and Fast serving. Keep it for every task unless the user explicitly requests another model.

`cursor-agent models` lists valid slugs for the account. The `system/init` event confirms the resolved display name.

## Permissions and approvals

- Auto Review evaluates Shell, MCP, and Fetch calls. A blocked operation is a result to report or solve within the existing controls.
- Do not retry with `--force`, `--yolo`, or `unrestricted`.
- Auto Review is a classifier, not isolation. Cursor's native sandbox remains disabled on this workstation because its AppArmor preflight fails.
- Project permissions live in `.cursor/cli.json`; global permissions live in `~/.cursor/cli-config.json`. Both `allow` and `deny` arrays are required.
- Keep allow rules narrow because they bypass classifier review. Rule syntax is `Shell(cmd)`, `Read(glob)`, `Write(glob)`, `WebFetch(domain)`, and `Mcp(server:tool)`.
- Use `permissions.json` only for recurring plain-language classifier guidance. It is not a security boundary.
- If Cursor fixes its AppArmor preflight, test `--sandbox enabled` before changing this default.
- Use `--approve-mcps` only when the task requires the configured MCP servers.
- Use `--mode plan` or `--mode ask` for untrusted input and read-only analysis.

A minimal global baseline is:

```json
{
  "version": 1,
  "editor": {"vimMode": false},
  "approvalMode": "auto-review",
  "permissions": {"allow": ["Shell(ls)"], "deny": []},
  "sandbox": {"mode": "disabled", "networkAccess": "user_config_with_defaults"}
}
```

## Installation

- Install with `curl https://cursor.com/install -fsS | bash`.
- Update with `cursor-agent update`.
- Authenticate with `cursor-agent login` or `CURSOR_API_KEY` in CI.
- Use absolute paths in prompts when the output location matters.
- Allow enough wall-clock time for the task. A trivial request may take around ten seconds; real work can take minutes.
