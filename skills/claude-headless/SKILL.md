---
name: claude-headless
description: Run Claude Code programmatically via the headless CLI (claude -p). Use when delegating a task to a separate Claude Code instance, scripting claude in shell or CI, or when the user mentions running Claude Code headless or programmatically.
---

# Claude Code headless (`claude -p`)

Workflow for driving Claude Code from a script or from another agent. Everything below was verified by actually running it.

## Core call

```bash
claude -p "<task>" --allowedTools "Read,Glob,Grep,Write,Edit" --permission-mode auto \
  --output-format json 2>err.log > out.json
```

- stdout carries exactly one JSON object — keep stderr redirected away from it.
- Read from the JSON: `.result` (final answer), `.session_id`, `.is_error`, `.num_turns`, `.permission_denials`, `.total_cost_usd`.
- Pipe input via stdin: `git diff | claude -p "review this diff"`.
- Exit codes: `0` success, `1` error or `--max-turns` hit, `143` killed. Exit `0` with `.is_error: true` happens (e.g. auth failure) — always check both.

## Gotchas

- **`--bare` breaks claude.ai subscription auth** — it fails instantly with "Not logged in" even when `claude auth status` says logged in. Skip `--bare` unless using API-key auth.
- **Put absolute paths in the prompt.** The child session has its own scratchpad; "create a file here" can land there instead of your cwd. Name explicit absolute target paths and verify the artifact exists after the call.
- **`--json-schema` output is a JSON *string* inside `.result`** — parse it: `jq -r '.result | fromjson'`.
- **Set generous timeouts.** A trivial repo question takes ~20 s; real work takes minutes.

## Multi-turn collaboration

Resumed sessions keep full context and answer much faster (no re-exploration):

```bash
sid=$(claude -p "Analyze X in this repo" --output-format json | jq -r .session_id)
claude -p "Follow-up: ..." --resume "$sid" --output-format json | jq -r .result
```

`--continue` resumes the most recent session in the cwd when you did not capture the id.

## Permissions

- **Default: `--permission-mode auto` with write access.** `--allowedTools "Read,Glob,Grep,Write,Edit" --permission-mode auto` — an agent that can create and edit files is far more useful, and `auto` removes routine prompts while a background classifier still blocks genuinely dangerous actions (`curl | bash`, force-push, production deploys, `rm -rf /`). Requires Plan/Team/Enterprise and a recent model (Sonnet 4.6+/Opus 4.6+/Fable 5). A blocked Write in `-p` mode fails silently, leaving the agent to describe work instead of doing it.
- Narrower fallbacks: `acceptEdits` auto-approves only file edits + basic FS commands; `dontAsk` denies everything outside `permissions.allow` — the docs' pick for locked-down CI.
- Restrict to read-only (`--allowedTools "Read,Glob,Grep"`) only in the rare cases that demand it: untrusted prompts/inputs, pure analysis of a repo that must stay pristine, audits.
- Scope Bash by prefix pattern: `--allowedTools "Bash(git diff *),Bash(ls *)"`.
- Tools outside the allowlist are silently denied in `-p` mode — check `.permission_denials` when the agent reports it could not do something.
- Reserve `--dangerously-skip-permissions` for disposable sandboxes.

## Structured output

```bash
git log --oneline -10 | claude -p "Classify each commit by conventional-commit type" \
  --output-format json \
  --json-schema '{"type":"object","properties":{"commits":{"type":"array","items":{"type":"object","properties":{"hash":{"type":"string"},"type":{"type":"string"}},"required":["hash","type"]}}},"required":["commits"]}' \
  | jq -r '.result | fromjson'
```

## Streaming and applications

- Live progress events: `--output-format stream-json --verbose --include-partial-messages` (newline-delimited JSON).
- Building an application rather than a shell script → use the Claude Agent SDK instead of the CLI: Python `uv add claude-agent-sdk`, TypeScript `npm install @anthropic-ai/claude-agent-sdk`. Docs: https://code.claude.com/docs/en/agent-sdk/overview
