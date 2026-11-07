# Observed in use

What happened in the user's real Codex runs and tests, mostly LAW critic and review runs launched from Claude Code. Each note has its date and the CLI version it was seen with. The CLI changes often, so a note is history, not a rule: if the current CLI behaves differently, the current behaviour wins, and the note should be updated or removed.

## 2026-09-20, codex-cli 0.155.1, GPT-5.6 Sol at low effort (research/claude-codex-headless-empirical-2026-09-20.md)

- Durations: web research about 2 minutes, bounded repository research 3–5 minutes, a task with subagents 5 minutes and more.
- A deadline in the prompt made the agent finish early with a normal `turn.completed` and a list of gaps. The wording: "Time budget: 10 minutes of wall-clock time from your first action. Run `date +%s` first and again after every few tool calls. When 8 minutes have passed, stop exploring and deliver the report with what you have, listing the areas you did not reach under a heading 'Not covered'." (The run used 4 and 3 minutes and finished in 164 s.)
- Asked for four subagents, Codex launched three ("the four-slot limit includes me"). Killed at 90 s, their work was lost; on resume the agent first claimed it could still reach them.

## 2026-10-03..10-12, codex-cli about 0.159–0.162 (LAW/WORKFLOW.md §1; the exact version per note was not recorded)

- With the prompt given as an argument and stdin left open, runs hung until stdin was redirected from `/dev/null`. The prompt piped on stdin did not show this.
- A relative `-o` path once produced no file; absolute paths worked. When `-o` pointed at the same report file the prompt asked Codex to write, the final message overwrote the report.
- "Model at capacity" errors went away on a retry about two minutes later.
- Review prompts worded with "bypass" or "tamper" were refused; the same review asked as a correctness question ran.

## 2026-10-12..10-16, codex-cli 0.162.1

- Around 150 waits written as repeated `sleep` / `pgrep` polling commands failed (mostly exit 144) or were stopped, across five LAW sessions. Waits that were one background command ending with the Codex process did not show this.
- Agents checked remaining quota by sending a throwaway "Reply with OK" run. The newest rollout already carries `rate_limits.primary.used_percent`; on 2026-10-16 it read 94.0 with `window_minutes` 10080, a weekly window. On 2026-10-14 the window reached 100% mid-batch and five runs ended with no output.

## 2026-11-07, codex-cli 0.166.0, gpt-6.1-sol at low effort

- A one-shot "Reply with the word ok" run outside any repository took 40 s and about 24,700 input tokens; the stream was `thread.started`, `turn.started`, one `item.completed` agent message, `turn.completed`. The rollout's `rate_limits` field was still present (weekly window, 10080 minutes).
