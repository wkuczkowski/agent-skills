---
name: diagnosing-bugs
description: Holds a diagnosis at reproducing and locating the failure before any code is changed. Use when the user says "diagnose" or "debug this", or reports something broken, throwing, failing or slow.
metadata:
  upstream: mattpocock/skills skills/engineering/diagnosing-bugs
  upstream-commit: "321658273cb1d20b76026717d027d505790106d4"
  adopted: "2026-09-22"
---

# Diagnosing bugs

Fixes attempted before the failure was reproduced and located have cost the user rounds of rework: a plausible cause is changed, the symptom stays, and the change itself then has to be unwound. Reproducing first has been slower to start and shorter overall.

Reproduced means one command the agent can run unattended that goes red on the symptom the user described, not on a nearby failure, and green once the bug is gone. A flaky bug counts once the rate is high enough to debug against; an intermittent failure that cannot be raised above the noise is worth saying out loud rather than working around.

Located means naming the file, function or boundary where the wrong value first appears, with the evidence that puts it there. A theory about which module is at fault is not a location.

When no loop can be built, the honest report is that: what was tried, and what would unblock it (access to an environment that reproduces, a redacted capture, permission to instrument). Guessing from there has been the expensive path.

The user's explicit instructions take precedence. When he asks for a fix straight away, say in one line what has not been reproduced yet and do as he asked.

Outputs shown to the user carry secrets: replace them with `<REDACTED>`, and build loops against environment variables so the credential never reaches the transcript.
