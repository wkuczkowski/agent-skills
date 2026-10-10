---
name: antigravity-headless
description: Use when you need a Gemini model, or another model offered through Google Antigravity, and are working outside the Antigravity harness. Runs Antigravity CLI (`agy`) headlessly with the model the task calls for.
---

# Antigravity headless (`agy -p`)

Drive Google Antigravity CLI non-interactively. Facts below were checked against `agy 1.3.3` on 2026-10-10; the docs and the measurements behind them are in the agent-skills repo under `research/antigravity-cli-headless-2026-10-10.md`. The online docs lag the CLI, so `agy --help` and `agy changelog` win where they disagree. The user's instructions take precedence over this skill.

A run is done when the result record says `status: "SUCCESS"`, `denied_actions` is absent, and `response` holds the answer.

## A one-shot run

```bash
timeout -s INT -k 30 3600 agy --model <slug> --output-format json \
  -p "$(cat prompt.txt)" </dev/null >out.json 2>err.log
```

- `-p` takes the prompt as its value, so it goes last or as `-p "<text>"`. `-p -` sends the literal "-" and stdin is ignored; a bare `-p` is a usage error. For a prompt too long for argv, pipe `jq -cn --rawfile t prompt.txt '{event:"user",message:{content:$t}}'` into `agy --input-format stream-json --output-format stream-json` (no `-p`).
- `agy` runs in the current directory; there is no `--cwd`. `--add-dir <path>` adds more roots.
- `--output-format stream-json` emits `init` (resolved `model`, `cwd`, `permission_mode`), `step_update` per step (tool name, parameters, errors) and a final `result`. Use it for anything longer than a quick question, so a stall is visible.
- `.conversation_id` from the result resumes the conversation with `--conversation <id>`; the id is not known before the run.
- `--json-schema <file|string>` adds `structured_output`; the schema root must be `"type":"object"`.
- `agy -p "/usage"` (also `/quota`, `/model`, `/skills`) answers without a model turn or quota spend.

## Model

`agy models` lists the slugs the account can use. The effort is part of the slug (`gemini-3.8-flash-high`, `-medium`, `-low`); the user's "Gemini Flash 3.8 High" is `gemini-3.8-flash-high`. Pass the slug alone: a suffixed slug plus a different `--effort` exits 1 with "conflicts with --effort", and a base slug with `--effort` shows only the base slug in `init`, so the effort cannot be confirmed. Without `--model` the run uses `model` from `~/.gemini/antigravity-cli/settings.json`. Confirm `init.model` matches the slug you asked for.

## Completion is not the exit code

- Exit 0 also covers a run that hit a permission denial or `--print-timeout`. Exit 1 is a startup or validation error (bad model, empty prompt), 2 a malformed stream input, 3 a model or API failure (an `AGY_ERROR:` line on stderr).
- A denied tool ends the turn at once: `status` stays `SUCCESS`, `response` is empty, `denied_actions` names the permission, and stderr says `no output produced`. Read the partial work from the `step_update` events.

## Permissions

Print mode never prompts. Reading and writing files inside the workspace and `search_web` run without approval; shell commands (`command`) and fetching a page (`read_url`) are denied unless `permissions.allow` in `~/.gemini/antigravity-cli/settings.json` has a matching rule (`command(git diff)`, `read_url(go.dev)`, `read_file(...)`, `write_file(...)`; deny beats ask beats allow). So:

- The user wants agy runs to execute commands, through allow rules rather than the bypass flag (decided 2026-10-10). `agy -p "/permissions"` lists the rules in force; a command outside them still ends the run, so name what the run may not do in the prompt ("no network, no installs"), or the model reaches for it and the run ends empty. The rules are prefix matches and not isolation: `python3` or `npm run` can do anything the user can.
- Changing the rules is the user's call; Claude Code's auto mode blocks an agent from editing that file.
- `toolPermission: "proceed-in-sandbox"` with `--sandbox` still denied commands in `-p` (tested 2026-10-10).
- `--dangerously-skip-permissions` approves everything; Claude Code's auto mode blocked launching it (2026-10-10).

## Context the run sees

agy loads `AGENTS.md`, `GEMINI.md` and `.agents/rules/*.md` from the working directory up to the repo root, global copies under `~/.gemini/`, and skills from `.agents/skills/` and `~/.gemini/antigravity-cli/skills/`. It does not read `CLAUDE.md`. No flag turns rules or skills off; `--disable-slash-commands` only stops `/name` in the prompt from expanding. Run from a scratch directory when the agent must not see a project.

## Time

The `timeout` guards a hung process; put a real deadline in the prompt. `--print-timeout` defaults to 0 (no limit) and exits 0 with partial output when it fires. A combined read-write-search prompt sat silent for 5 minutes with JSON output on 2026-10-10 while each part alone finished in seconds; a stream showed where single steps went, so watch long runs with `stream-json` and judge a hang by a stream that stops growing.

Auth is the Google sign-in cached in the keyring; `Authentication expired. Please log back in.` means the user has to run `agy` interactively once.
