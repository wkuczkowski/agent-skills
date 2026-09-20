---
name: cursor-headless
description: Use when you need the Grok model available through Cursor and are working outside the Cursor harness. Runs Cursor Agent headlessly with Grok 4.6 High, in normal or Fast mode depending on the task.
---

# Cursor headless (`cursor-agent -p`)

Drive Cursor Agent CLI non-interactively. These instructions target `cursor-agent 2026.09.18-9a7762b`; the measurements behind them are in the agent-skills repo under `research/cursor-headless-empirical-2026-09-20.md`.

## Choose a run

Use this for a one-shot task:

```bash
sid=$(cursor-agent create-chat)
timeout -k 15 3600 cursor-agent -p --trust --auto-review --sandbox disabled \
  --workspace "$PWD" --model cursor-grok-4.6-high --resume "$sid" \
  --output-format stream-json < prompt.txt >events.jsonl 2>err.log
```

- `create-chat` gives the session id before the run starts, so it survives any failure. Keep it next to the stream.
- `--trust` is mandatory in headless mode.
- Always pass `--auto-review --sandbox disabled` and the model explicitly.
- Pass the prompt on stdin from a file, and keep the file. One argv string is capped at 128 KiB, so a long brief passed as an argument fails. With no prompt argument, stdin is the whole prompt; with one, piped stdin is appended to it, so redirect stdin from `/dev/null` then.
- `--workspace <path>` sets the working root. It does not isolate files: the session reads and writes any absolute path on disk. Keep secrets and answer keys off reachable paths, name allowed paths in the task, and use absolute paths when the output location matters.
- For follow-up turns, subagents, parallel runs, monitoring, or runs launched by a Claude Code orchestrator, read [sessions, multitasking, and monitoring](references/sessions-multitasking-monitoring.md).
- For MCP, read [MCP servers](references/mcp.md) before constructing the command.
- Cursor runs hooks in headless mode. Before adding one, or when a `hooks.json` exists in the workspace, read [hooks](references/hooks.md).

Cursor writes client state under `~/.cursor/projects`. If a parent sandbox blocks startup, allow the outer process to write there. Keep Cursor's own Auto Review setting and task scope unchanged.

If the CLI itself fails, hangs, selects the wrong model, emits malformed output, or cannot initialize state or permissions, read [problem reporting](references/problem-reporting.md). Record the sanitized failure, tell the user where the report is, and continue with unaffected work.

## Time

A killed run loses every in-flight tool call and all in-flight subagent work, so arrange for the run to end on its own.

- The `timeout` is a guard against a hung process, not a schedule. Measured on Grok 4.6 High: a focused question 1–3 minutes, web or repository research 3–8 minutes, a broad task with subagents 8–15 minutes and more. Set the ceiling at several times the expected duration; 3600 s is a sound default. Run anything beyond a few minutes in the background.
- When a deadline exists, put it in the prompt rather than in the `timeout`. This wording made the agent finish early with a normal `result` and a list of gaps:

  > Time budget: 10 minutes of wall-clock time from your first action. Run `date +%s` first and again after every few tool calls. When 8 minutes have passed, stop exploring and deliver the report with what you have, listing the areas you did not reach under a heading "Not covered".

  The agent errs early (a 4-minute budget ended after 1.5–2 minutes), so state the budget you can really afford.
- For work expected to pass ten minutes, ask for a progress file: findings appended to an absolute path after each area, and the final report in a second file. Partial results then exist whatever happens to the process.
- Judge a hang by silence, not by elapsed time. A healthy stream pauses up to about 4 minutes while subagents work or a long reply is generated. Ten minutes without growth in `events.jsonl` is a hang: kill it and resume.
- If a run dies anyway (exit 124 from `timeout`, or no `result` event), resume the same session with a message that names the prompt file and the progress file by absolute path and asks to continue. Completed steps are still in the session; a run killed in its first seconds has lost even the prompt, which is why the message points at the file.

## Output and completion

- A finished run exits 0 and ends the stream with a `result` event. The CLI emits `result` only with `subtype: "success"`; failure shows as a non-zero exit, stderr, and a missing `result`. Keep stderr separate from stdout.
- Confirm `system/init.model` matches the slug you selected, Fast or not. `system/init.permissionMode` may report `default` despite `--auto-review`; the captured command is the review-mode record.
- `.result` concatenates the text of every assistant turn without separators. Extract the last assistant text when exact output matters:

  ```bash
  jq -rs '[.[] | select(.type=="assistant") | .message.content[]?
           | select(.type=="text") | .text] | last' events.jsonl
  ```

- When the task asks for a result file and the file is missing, extract the content from that last assistant text. The model may emit it in the reply with no tool calls.
- Cursor has no structured-output schema flag. Ask for JSON when needed, but validate it in the caller.

## Model

Two slugs, same model and effort: `cursor-grok-4.6-high` (normal) and `cursor-grok-4.6-high-fast` (Fast, quicker output). Pick per run. Fast fits when a quicker answer helps: you or the user are waiting on the result, the run is one step in an interactive loop, or it is a smoke check. Normal fits background and batch work where nobody is blocked on the answer. The user's choice of mode takes precedence.

`cursor-agent models` lists valid slugs for the account. The `system/init` event confirms the resolved display name.

## Required client configuration

Two keys in `~/.cursor/cli-config.json` decide behaviour that no flag controls. Check them before a run that depends on web search or on delegation:

```bash
jq '{autoAcceptWebSearch, explore: .subagentModels.explore}' ~/.cursor/cli-config.json
```

- `autoAcceptWebSearch: true`. Otherwise every web search is rejected in print mode and the agent falls back to fetching guessed URLs.
- `subagentModels.explore: "inherit"` (with `exploreSubagentModel: "inherit"`). Otherwise subagents run on `composer-2.5-fast` instead of the Grok slug you selected.

If either differs, tell the user; a CLI update may have reset it.

## Permissions and approvals

- Auto Review evaluates Shell, MCP, and Fetch calls. A blocked operation is a result to report or solve within the existing controls. List what was rejected in a run with:

  ```bash
  jq -c 'select(.type=="tool_call" and .subtype=="completed") | .tool_call | to_entries[0]
         | select(.value.result.rejected) | {tool: .key, args: .value.args}' events.jsonl
  ```

- Do not retry with `--force`, `--yolo`, or `unrestricted`.
- Auto Review is a classifier, not isolation. Cursor's native sandbox remains disabled on this workstation because its AppArmor preflight fails.
- Project permissions live in `.cursor/cli.json`; global permissions live in `~/.cursor/cli-config.json`. Both `allow` and `deny` arrays are required.
- Keep allow rules narrow because they bypass classifier review. Rule syntax is `Shell(cmd)`, `Read(glob)`, `Write(glob)`, `WebFetch(domain)`, and `Mcp(server:tool)`.
- Use `permissions.json` only for recurring plain-language classifier guidance. It is not a security boundary.
- Use `--approve-mcps` only when the task requires the configured MCP servers.
- Use `--mode plan` or `--mode ask` for untrusted input and read-only analysis.

Update the CLI with `cursor-agent update`, then re-check the two configuration keys above.
