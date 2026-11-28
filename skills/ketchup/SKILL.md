---
name: ketchup
description: Catches the user up on what changed in this chat since his last message and lists every decision waiting on him. Use when he types /ketchup or says "ketchup" (dictation also writes "keczup", "keczap") as the request itself.
metadata:
  upstream: cursor/plugins pstack/skills/ketchup/SKILL.md
  upstream-commit: "bc1443790cc980d0ea857663260a831299884508"
  adopted: "2026-11-28"
---

# Ketchup

The user was away from this chat and wants two things from one reply: what changed since the last message he wrote, and every call that waits on him, each decidable from the reply alone. Assume he remembers his last message and nothing after it; notifications and subagent results are not his messages. His instructions take precedence over this skill.

Adopted 2026-11-28 after he had asked whether work was still running or what had been achieved by hand about nine times in two weeks and got answers in a different shape each time.

## The turn is read-only

Nothing is changed, started or posted in this turn: no code edits, no app or job launched, no pushes. Visuals come only from what already exists. Before writing, check the current state of each thing this chat touched (background jobs, delegates, subagents, branches, PRs) rather than reporting from memory, and check each number against the fact it comes from.

## What the reply holds, in this order

1. **One sentence**: what waits on him, or that nothing does. If nothing happened and nothing waits, that sentence is the whole reply.
2. **What happened**, pictures first: the screenshots and videos the work produced, embedded inline, then any diagram that explains a change. Then one line per thing he asked for: what came of it, and whether it is done, still running, failed or not checked. A line stays true when read alone; nothing still waiting on him is marked done. Work that changed nothing for him is left out. Under about 100 words, pictures aside.
3. **What waits on him**: only calls or steps he alone can take (a product choice, an approval, an irreversible action, access the agent lacks), including any raised before his last message. Work the agent can still do is not one, and none is invented to fill the list. Each item: the call, your pick, what it changes for him, the evidence behind the pick, its risk even when low. Items that share one call are listed under it, riskiest first. For a choice between options, every option the work weighed with what it gets him and what it costs. An unknown fact a call needs is written as "unknown". The list is never shortened to a count or a link.

## How it reads

Where the `reporting-to-the-user` skill is available it holds his style and goes first. Without it: his language, plain words, short sentences, what changed for him rather than how it was done ("the tests pass", not "CI is green"); things are called by the names he uses, and hashes, paths, commands and test counts appear only when he needs one to act.

## Visuals

A visual is a picture: a screenshot, a video, a diagram, a chart. A Markdown table is not one. Draw what is easier to grasp as a picture than as sentences: how a design works, a chain of dependent changes with each one's state, a before and after, options side by side. A status list or a list of action items is not drawn. A visual stays readable at a glance (about six boxes, a few plain words each) and no fact is given twice.

A visible change with no screenshot gets "no screenshot yet" on its line plus what changed on screen; a screen that was never captured is never drawn or generated. In T3 Code, images embed by absolute path and a diagram or chart is shown with `html_preview` then `html_render`; elsewhere a Mermaid block where the harness renders it.
