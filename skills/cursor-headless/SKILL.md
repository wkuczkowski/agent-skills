---
name: cursor-headless
description: Use when you need the Grok model available through Cursor and are working outside the Cursor harness. Runs Cursor Agent headlessly with Grok 4.7 High, in normal or Fast mode depending on the task.
---

# Cursor headless (`cursor-agent -p`)

Facts below were checked on 2026-11-07 against `cursor-agent 2026.10.22-27dcf5f` (`--help`, the installed bundle, the CLI changelog) unless dated otherwise. What was seen in real runs, with dates and versions, is in [observed in use](references/observed-in-use.md); evidence in the agent-skills repo under `research/cursor-headless-*.md`.

## One-shot run

```bash
sid=$(timeout 60 cursor-agent create-chat)
timeout -k 15 3600 cursor-agent -p --trust --auto-review --sandbox disabled \
  --workspace "$PWD" --model grok-4.7-high --resume "$sid" \
  --output-format stream-json < prompt.txt >events.jsonl 2>err.log
```

- `create-chat` returns the session id before the run starts (it worked even with an expired login, 2026-11-07), so the id survives any failure. Keep it next to the stream. It once hung right after a login (2026-11-07), hence its own `timeout`.
- `--trust` is required in print mode; without it the run exits asking for it.
- `--auto-review --sandbox disabled` and an explicit `--model` on every run (the user's choice). Never `--force`, `--yolo`, or `unrestricted`, and never retry with them. Cursor's headless docs say `--force` is needed for edits; under `--auto-review` edits run without it.
- Cursor's native sandbox stays disabled: its AppArmor preflight fails on this workstation. Auto Review is a classifier, not isolation.
- Prompt on stdin from a file, and keep the file. One argv string is capped at 128 KiB. With no prompt argument, stdin is the whole prompt; with one, piped stdin is appended, so redirect stdin from `/dev/null` then.
- `--workspace` sets the working root but isolates nothing: the session reads and writes any absolute path, and since 2026.10.15 Grep and Glob also search outside the workspace. Keep secrets and answer keys off reachable paths and name the allowed paths in the task.
- `--output-format json` prints nothing until the end, so a killed run leaves no output; use `stream-json`.
- A killed run loses every in-flight tool call and subagent, so the `timeout` is a hang guard set well above the expected duration, and a deadline goes in the prompt. Resume the same `$sid` with a message naming the prompt file (and any progress file) by absolute path: a run killed in its first seconds has not stored even the prompt.

Read when needed: [sessions, subagents, parallel runs, monitoring, Claude Code orchestration](references/sessions-multitasking-monitoring.md); [MCP](references/mcp.md) before a run that needs an MCP server; [hooks](references/hooks.md) before adding one or when a `hooks.json` exists in the workspace.

## Output and completion

- Success is exit 0 and a final `result` event. The CLI emits `result` only with `subtype: "success"`; failure is a non-zero exit, stderr, and no `result`. Keep stderr separate.
- `system/init.model` is a display name, not the slug: `grok-4.7-high-fast` showed `Grok 4.7 256K High Fast` (2026-11-07). It differs from the display text in `cursor-agent models` (`Grok 4.7  High Fast` plus two zero-width spaces), so check that it contains `4.7`, `High`, and `Fast` or not, rather than comparing strings. `system/init.permissionMode` is hard-coded `default`; the captured command is the record of `--auto-review`.
- `.result` concatenates every assistant turn without separators. When exact output matters, take the last assistant text:

  ```bash
  jq -rs '[.[] | select(.type=="assistant") | .message.content[]?
           | select(.type=="text") | .text] | last' events.jsonl
  ```

- When a requested result file is missing, the content may be in that last text instead.
- There is no structured-output flag. Ask for JSON and validate it in the caller.

## Model

`grok-4.7-high` (normal) and `grok-4.7-high-fast` (Fast, quicker output): same model and effort, chosen by the user on 2026-10-10. Fast when someone is waiting on the answer or for a smoke check; normal for background and batch work. The user's choice of mode for a run takes precedence. `cursor-agent models` lists the slugs the account has; both were listed on 2026-11-07.

## Client configuration no flag controls

```bash
jq '{autoAcceptWebSearch, explore: .subagentModels.explore, exploreSubagentModel}' ~/.cursor/cli-config.json
```

- `autoAcceptWebSearch: true`, or every web search is rejected in print mode and the agent falls back to guessed URLs.
- `subagentModels.explore` and `exploreSubagentModel` both `"inherit"`, or explore subagents run on Composer instead of the Grok slug.

All three were set on 2026-11-07, after the update to 2026.10.22. If one differs, tell the user rather than editing it; a CLI update may reset it. Re-check after `cursor-agent update`.

## Permissions

- Auto Review rejects some Shell, MCP and Fetch calls. A rejection is a result to report or work around within the controls. List them:

  ```bash
  jq -c 'select(.type=="tool_call" and .subtype=="completed") | .tool_call | to_entries[0]
         | select(.value.result.rejected) | {tool: .key, args: .value.args}' events.jsonl
  ```

- Allow rules bypass the classifier, so keep them narrow: `.cursor/cli.json` (project) or `~/.cursor/cli-config.json` (global), with both `allow` and `deny` arrays present. Syntax: `Shell(cmd)`, `Read(glob)`, `Write(glob)`, `WebFetch(domain)`, `Mcp(server:tool)`. Changing them is the user's call.
- `--mode plan` or `--mode ask` makes a run read-only, for untrusted input or pure analysis.

## When the CLI itself fails

- `Authentication required` on stderr, exit 1, empty stdout, no `system/init`: the login expired (tokens in `~/.config/cursor/auth.json`; seen 2026-11-07). Only the user can fix it with `cursor-agent login`, which opens a browser. `cursor-agent status` still printed "Logged in" in that state; `cursor-agent models` shows the real state.
- Cursor writes client state under `~/.cursor/projects`; if a parent sandbox blocks startup, allow the outer process to write there without widening the task.
- For any CLI or harness failure (auth, models, transport, trust, malformed events, crashes, stalls), follow [problem reporting](references/problem-reporting.md), tell the user where the report is, and continue with unaffected work.
