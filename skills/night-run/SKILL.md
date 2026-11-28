---
name: night-run
description: Runs a long task unattended until a stated hour, with questions and unlocks first, silence during, a report at the hour. Use when the user says "tryb noc" or "tryb nocny" ("tryb noc do 7:00"), is going to sleep or away while work continues, or wants a report at a set hour with no contact until then. Also on "wstrzymaj" or "pauza" during such a run.
---

# Night run

The user leaves and the agent carries a task alone until an hour he names. He had been retyping the same terms by hand, about nine times in the two weeks to 2026-11-28 (questions now, then silence, a report at 7:00, limits may be used up); this skill is those terms. Whatever he says at the start (an hour, a cap, a scope, a model split) replaces the defaults here, and his instructions take precedence over this skill.

Ideas taken from tstack `full-send` (one phrase that means "I am not watching") and pstack's overnight guide (a checkable finish condition, an escape hatch). Not taken: night mode grants no permission the session did not already have. It does not merge on his behalf where he has not allowed it, and it overrides no project rule.

## Before he leaves: one message

- The goal in your own words and what done looks like, as checks that can be run during the night.
- Every question at once. One that blocks all work is asked and waited on. For the rest, state the assumption you will work under and start; he corrects it if he is still there.
- Everything only he can unlock, found by thinking the night through to its end: passwords and sudo, permission prompts the harness will raise (choose routes that do not prompt; the global instructions list the known ones), a logged-in browser session, a network switch when a source rate-limits by address (he switched networks by hand about twelve times in one project, 2026-11-14..16), usage limits or quota about to run out.

Runs stalled on a prompt or a manual unlock on 2026-10-21, 10-29 and 11-03; each cost the whole night.

## During the night

- No questions and no status messages. A message mid-run reads as a stop, and nobody answers it.
- Ending the turn ends the night: nothing prompts the agent again unless a wake mechanism is armed (a background task's notification, a delegate's completion, a scheduled wakeup). Waiting on long checks is covered by `testing-and-shipping` where that skill is available.
- Blocked on one thing: note it and take the next independent thing. An agent idling for hours on one blocked item was a complaint of his on 2026-11-27.
- A decision that would normally be his and is reversible: make it, and add it to the list of decisions made for him. One that is irreversible or outward-facing and not already granted waits for the report.
- State lives in the project's handoff file (where the project keeps one; otherwise a dated file next to its docs), updated at each milestone: goal, done-checks, what is finished with its proof, what is in progress, decisions made, what is blocked. Sessions get cut by limits, account switches and restarts; the next session resumes from that file.
- Stuck on everything for hours: stop and write why. The goal is not reinterpreted and the done-checks are not relaxed to declare success.
- He may be asleep next to the laptop: browsers run headless and muted, and heavy jobs stay inside `local-machine-limits`.

## Defaults (user, 2026-11-28)

- **Finished before the hour**: take the next items from the project's own list (plan, backlog, handoff), in its order, until the hour. With no such list, spend the time checking the night's work where its proof is thinnest rather than inventing scope.
- **Limits**: usage may be spent in full unless he names a cap.
- **Report**: in the chat, with pictures where they help; a standalone page or artifact only when he asks for one.

## At the hour

The report is ready by the hour he named, so new work stops early enough to verify and write. It holds: what is done and how each part was checked; what failed or was not checked; the decisions made for him, each with its reason; what waits on him, each with your pick. `reporting-to-the-user` holds how he reads such a reply. The handoff file carries the same facts in case the session is gone by morning.

## Pause

"Wstrzymaj" or "pauza" means now: finish or back out the current step, stop the subagents and delegates too, checkpoint (commit or handoff note), and confirm in one line what stopped and what, if anything, is still running. On 2026-11-26 subagents kept working after he had asked for a pause. "Wznów" picks up from the checkpoint.
