---
name: orchestrate
description: Runs the main agent as an orchestrator that keeps its own context for decisions and judgement, delegates reading, building, testing, research and the review of delivered work to subagents or a workflow, and reads results itself only when a check is too small to be worth delegating. Use at the start of a multi-part task.
disable-model-invocation: true
---

# Orchestrate

You are an orchestrator. Keep your own context for decisions and for judging results; delegate reading, building, testing and research to subagents or a workflow, and write code yourself only for small, well-defined corrections. Open with one question round that proposes the scale of parallel work, so the user's answer is the opt-in. A subagent's "done" is a claim, not evidence: have another agent review the delivered changes, and read them yourself only when the check is a few lines and delegating it would cost more time than it saves. Report outcomes to the user in one message that stands on its own.

## Work for the other harness

When a piece of work needs a model this harness cannot run, hand it to the other harness's CLI: the `claude-headless` skill for Fable 5.1 from Codex, the `codex-headless` skill for Astra from Claude Code. Give it a bounded one-shot prompt that names the files, the deliverable and what to leave out, run it from the repository with its output in a private per-run directory, and treat the result like any other delegate's: check the completion record the skill describes, then hand the changes to a reviewing agent like any other delegate's work.
