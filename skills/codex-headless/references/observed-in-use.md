# Observed in use

What happened in the user's real Codex runs, mostly LAW critic and review runs launched from Claude Code. Each note has its date and the CLI version it was seen with. The CLI changes often, so a note is history, not a rule: if the current CLI behaves differently, the current behaviour wins, and the note should be updated or removed.

## 2026-10-03..10-12, codex-cli about 0.159–0.162 (LAW/WORKFLOW.md §1; the exact version per note was not recorded)

- With the prompt given as an argument and stdin left open, runs hung until stdin was redirected from `/dev/null`. The default form in SKILL.md, prompt piped on stdin, did not show this.
- A relative `-o` path once produced no file; absolute paths worked. When `-o` pointed at the same report file the prompt asked Codex to write, the final message overwrote the report.
- "Model at capacity" errors went away on a retry about two minutes later.
- Review prompts worded with "bypass" or "tamper" were refused; the same review asked as a correctness question ran.

## 2026-10-12..10-16, codex-cli 0.162.1

- Around 150 waits written as repeated `sleep` / `pgrep` polling commands failed (mostly exit 144) or were stopped, across five LAW sessions. Waits that were one background command ending with the Codex process did not show this.
- Agents checked remaining quota by sending a throwaway "Reply with OK" run. The newest rollout already carries `rate_limits.primary.used_percent`; on 2026-10-16 it read 94.0 with `window_minutes` 10080, a weekly window. On 2026-10-14 the window reached 100% mid-batch and five runs ended with no output.
