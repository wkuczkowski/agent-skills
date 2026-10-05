---
name: grilling
description: Stress-tests a plan, decision or idea with the user, putting to him only the decisions that are his and settling the rest itself. Use when the user asks to be grilled or to have a plan stress-tested.
disable-model-invocation: true
metadata:
  upstream: mattpocock/skills skills/productivity/grilling
  upstream-commit: "85f83d3fde1d3a90d5c9a657f6998c79a6c37308"
  adopted: "2026-10-05"
---

# Grilling

The aim is a plan the user and the agent understand the same way, with every branch of it decided. Most branches are not his to decide, and treating them as his has been the main cost of grilling so far.

## What the record shows (sessions 2026-09-04 to 2026-10-04)

- Across 30 question rounds and 168 answers, 53% of answers were a bare "zgoda", "ok" or "zgodnie z rekomendacją", and the longest rounds ran to 18–32 questions. Twice he stopped mid-round and handed the rest back ("Możesz podejmować sam decyzje", 2026-09-23; Q16–18 to be settled by the agent with a critic, 2026-10-03).
- What he waved through: engineering choices (cache TTLs, formatters, refactors, commit grouping, CI design, parser edge cases), repo and tool mechanics, facts that could be looked up, and legal interpretation, which he says he cannot judge.
- What he engaged with: his own preferences and UX, business and organisational facts only he knows, scope and direction, conventions, money and model budget. These are also where his answers changed the recommendation or added something new.
- He asks to be taught before deciding something unfamiliar ("wytłumaczył tak abym mógł … podjąć dobrą decyzję"), and he expects the agent to bring a better idea when it sees one.

## How the open points split

- **Facts** about the code, the environment, the docs or the law: found by the agent or a subagent, never asked. A legal question goes to research and a critic, not to him.
- **Decisions the agent can carry**: technical and reversible choices with a defensible answer. The agent decides, and on a contested one a critic subagent with fresh context checks it. They reach the user as a short "decided" list he can override, not as questions.
- **His decisions**: preferences, what he or the firm wants, scope and direction, money, anything visible to clients or colleagues, and steps no one can undo. Only these are questions.

## Rounds

- A round holds the few questions whose answers unblock the most, each with a recommendation and its reason, answerable in a word. A question that depends on another still open waits for the next round.
- An unfamiliar concept gets one plain sentence of explanation inside the question.
- The round also carries the decided list since the last round, a line per item, and any better route the agent sees.
- Questions go in the user's language.

The session ends when no decision of his is open. A short summary of what was decided, by whom, closes it, and the work waits for his go-ahead. The user's instructions take precedence over this skill: when he says to decide on your own, that covers the rest of the session.
