# Observed in use

What happened in real Cursor runs, each note with its date, CLI version and model. The CLI and models change often, so a note is history, not a rule: if the current CLI or model behaves differently, the current behaviour wins, and the note should be updated or removed.

## 2026-09-20, cursor-agent 2026.09.18, Grok 4.6 High (normal and Fast slugs)

Source: `research/cursor-headless-empirical-2026-09-20.md`. On this version `--model` could silently run the normal slug when the Fast one was asked for (fixed in 2026.09.28), so "Fast" runs here may have been normal ones.

- Durations: a focused question 1–3 minutes, web or repository research 3–8 minutes, a broad task with subagents 8–15 minutes and more. Three parallel runs on the normal slug took 195, 316 and 414 s. Grok 4.7 has not been timed.
- The stream paused up to about 4 minutes while subagents worked (each 2.5–4 minutes) or a long reply was generated.
- A wall-clock budget in the prompt made the agent finish early with a normal `result` and a list of gaps; it erred early (a 4-minute budget ended after 1.5–2 minutes). The wording used: "Time budget: 10 minutes of wall-clock time from your first action. Run `date +%s` first and again after every few tool calls. When 8 minutes have passed, stop exploring and deliver the report with what you have, listing the areas you did not reach under a heading 'Not covered'."
- A prompt asking for findings appended to an absolute-path progress file and the report in a second file left partial results after a `timeout` kill; resuming with a message naming both files finished the work without relaunching subagents.

## 2026-11-07, cursor-agent 2026.10.22

- The login had expired (tokens dated 2026-11-03). `create-chat` still returned an id; `cursor-agent -p` exited 1 with `Authentication required` on stderr and an empty stdout; `cursor-agent status` printed "Logged in (unable to fetch user details)"; `cursor-agent models` failed with the same error. After the user's `cursor-agent login`, the first `create-chat` hung for over 5 minutes with no output; a retry returned in 2 s. Wrapping it in `timeout 60` costs nothing.
- Smoke run, `grok-4.7-high-fast`, prompt "Reply with the word ok": exit 0 in 8 s wall clock (`duration_ms` 4234); events `system/init`, `user`, five `thinking`, one `assistant`, `result` (`subtype: success`, `result: "ok"`); `init.session_id` equalled the `create-chat` id; `init.model` was `Grok 4.7 256K High Fast`; the last-assistant-text jq returned `ok`.
- `~/.cursor/cli-config.json` default model read "Grok 4.7 256K Low" (it read "High Fast" on 2026-10-10). Runs that pass `--model` are unaffected.
