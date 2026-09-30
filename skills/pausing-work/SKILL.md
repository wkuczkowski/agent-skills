---
name: pausing-work
description: Brings running work to a safe stop and leaves a handoff the next session can resume from. Use when the user invokes it before closing the laptop, pausing a long run, running out of usage, or starting a new session.
disable-model-invocation: true
---

# Pausing work

The user often stops a long run on short notice: the laptop closes in 15 minutes, the session will not resume, the usage limit is near. What has gone wrong before: the stop took longer than the time he gave, and background tasks kept running after the agent said it had stopped ("20 running tasks").

1. Take the deadline from the message and keep inside it; a partial handoff on time is worth more than a complete one late. Done when the reply arrives before the deadline.
2. Ask each running delegate to reach a safe point and write its state to a file, then stop background tasks, monitors and servers the work started. Done when the harness task list and `ps` show nothing left from this work, or the reply names what is still running and why.
3. Write or replace one handoff file in the project's known place (`HANDOFF-<date>.md` beside the plan, or where the project keeps them): done, in progress with the exact point it stopped, next steps in order, open decisions for the user, and how to resume. Done when a fresh agent could resume from that file alone.
4. When the user asks for a prompt for a new session, write it to a file next to the handoff and show it; it points at the handoff and repeats the user's standing instructions for the run (models, agent cap, reporting). Done when the prompt is copy-paste ready.
5. Reply in a few lines: stopped or not, where the handoff is, what he has to decide. A summary he will read later has worked well as an artifact.

The user's instructions take precedence over this skill.
