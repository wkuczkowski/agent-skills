---
name: antigravity-headless
description: Use when you need a Gemini model, or another model offered through Google Antigravity, and are working outside the Antigravity harness. Runs Antigravity CLI (`agy`) headlessly with the model the task calls for.
---

# Antigravity headless (`agy -p`)

Drive Google Antigravity CLI non-interactively. Facts below were checked against `agy 1.4.12` on 2026-11-07 unless dated otherwise; evidence is in the agent-skills repo under `research/antigravity-cli-headless-2026-10-10.md` and `research/antigravity-headless-recheck-2026-11-07.md`. `agy --help` wins over this skill. `agy changelog` in 1.4.12 stops at 1.3.3; the 1.4.x notes are only in the CLI section of https://antigravity.google/docs/changelog. The user's instructions take precedence over this skill.

A run is done when the result record says `status: "SUCCESS"`, has no `finished_reason` and no `denied_actions`, and `response` holds the answer.

## A one-shot run

```bash
timeout -s INT -k 30 3600 agy --model <slug> --output-format json \
  -p "$(cat prompt.txt)" </dev/null >out.json 2>err.log
```

- `-p` takes the prompt as its value, so it goes last or as `-p "<text>"`; a bare `-p` exits 2 with `flag needs an argument`. `-p -` sent the literal "-" and ignored stdin (1.3.3). For a prompt too long for argv, pipe `jq -cn --rawfile t prompt.txt '{event:"user",message:{content:$t}}'` into `agy --input-format stream-json --output-format stream-json` (no `-p`).
- `agy` runs in the current directory; there is no `--cwd`. `--add-dir <path>` adds more roots.
- `--output-format stream-json` emits `init` (resolved `model`, `cwd`, `permission_mode`, and `conversation_id` at the top level), `step_update` per step (tool name, parameters, errors) and a final `result`. Use it for anything longer than a quick question, so a stall is visible.
- `conversation_id` resumes the conversation with `--conversation <id>`; it cannot be chosen beforehand.
- `--json-schema <file|string>` adds `structured_output`; the schema root must be `"type":"object"`.
- `agy -p "/usage"` (also `/quota`, `/model`, `/skills`, `/permissions`) answers without a model turn or quota spend.

## Model

`agy models` lists the slugs the account can use; on 2026-11-07 Gemini was down to `gemini-3.8-flash-{high,medium,low}` (no Pro), plus Claude and GPT-OSS slugs. The effort is part of the slug; the user's "Gemini Flash 3.8 High" is `gemini-3.8-flash-high`. Pass the slug alone: a suffixed slug plus a different `--effort` exits 1 with "conflicts with --effort", and a base slug with `--effort` showed only the base slug in `init` (1.3.3), so the effort could not be confirmed. Without `--model` the run uses `model` from `~/.gemini/antigravity-cli/settings.json`. Confirm `init.model` matches the slug you asked for.

## Completion is not the exit code

- Exit 0 also covers a run that hit a permission denial. Exit 1 is a startup or validation error (unknown model, effort conflict, empty prompt), 2 a usage error or malformed stream input, 3 a model or API failure (an `AGY_ERROR:` line on stderr).
- `finished_reason` (since 1.4.11) appears only when the turn ended early: `print_timeout` (partial response), `interrupted`, `error`. Whether a `--print-timeout` expiry still exits 0 on 1.4.12 is untested; the web changelog says 1.5.0 makes it exit 3.
- A denied tool ends the turn at once: `status` stays `SUCCESS`, `response` is empty, `denied_actions` names the permission, and stderr says `no output produced` (1.3.3). Read the partial work from the `step_update` events.

## Permissions

Print mode never prompts. Reading and writing files inside the workspace and `search_web` run without approval; shell commands (`command`) and fetching a page (`read_url`) are denied unless `permissions.allow` in `~/.gemini/antigravity-cli/settings.json` has a matching rule (`command(git diff)`, `read_url(go.dev)`, `read_file(...)`, `write_file(...)`; deny beats ask beats allow). So:

- The user wants agy runs to execute commands, through allow rules rather than the bypass flag (decided 2026-10-10). `agy -p "/permissions"` lists the rules in force; a deny-listed command fails as a tool error and the run carries on, but a command matching no rule still ends the run empty, so name what the run may not do in the prompt ("no network, no installs"), or the model reaches for it and the run ends empty. The rules are prefix matches and not isolation: `python3` or `npm run` can do anything the user can.
- Changing the rules is the user's call; Claude Code's auto mode blocks an agent from editing that file.
- `--settings '<json>'` applies settings.json keys to one session without saving them. A `permissions.allow` or `permissions.deny` list given there replaces that whole list for the run instead of adding to it: passing one extra deny rule dropped the user's 18 deny rules (`sudo`, `git push`, `curl`, ...) from `/permissions` (2026-11-07).
- `toolPermission: "proceed-in-sandbox"` with `--sandbox` still denied commands in `-p` (tested 2026-10-10).
- `--dangerously-skip-permissions` approves everything; Claude Code's auto mode blocked launching it (2026-10-10).

## Context the run sees

agy loads `AGENTS.md`, `GEMINI.md` and `.agents/rules/*.md` from the working directory up to the repo root, global copies under `~/.gemini/`, and skills from `.agents/skills/` and `~/.gemini/antigravity-cli/skills/`. It does not read `CLAUDE.md`. No flag turns rules or skills off; `--disable-slash-commands` only stops `/name` in the prompt from expanding. Run from a scratch directory when the agent must not see a project. (Docs as of 2026-10-10; not re-tested on 1.4.12.)

## Time

The `timeout` guards a hung process; put a real deadline in the prompt. `--print-timeout` defaults to 0 (no limit). A combined read-write-search prompt sat silent for 5 minutes with JSON output on 2026-10-10 while each part alone finished in seconds; a stream showed where single steps went, so watch long runs with `stream-json` and judge a hang by a stream that stops growing. A one-word answer from `gemini-3.8-flash-low` took 17 s on 2026-11-07, so a few seconds of silence is not a hang.

Auth is the Google sign-in cached in the keyring; `Authentication expired. Please log back in.` means the user has to run `agy` interactively once.
