---
name: research
description: Investigates a question against high-trust primary sources and writes the findings into the repo for other agents to use. Use when the user wants a topic researched, docs or API facts gathered, or reading legwork delegated to subagents.
metadata:
  upstream: mattpocock/skills skills/engineering/research
  upstream-commit: "5863806dd4e15c63174bee4e7515a4256792ecfa"
  adopted: "2026-09-20"
---

# Research

Research the question and leave findings in the repo that another agent can build on without rechecking them. The user's instructions take precedence over this skill.

**Delegate the reading to subagents**, so your context stays free and you keep working while they read. How many, how the topic is split and who assembles the result is yours to decide: a narrow question fits one subagent, a topic with independent threads fits several in parallel.

**Work from primary sources**: whoever owns the claim (official docs, source code, a spec, a first-party API, the original study, the legal text), not a write-up of it. Follow each claim back to that source and cite it.

**Done means verified.** Check every claim the findings rest on against its source and mark what remains unconfirmed. If a missing source could change the recommendation, name the conclusion that depends on it. Revisit that conclusion when the source becomes available. On a complex topic, consider asking a subagent who did not gather the evidence to check the key claims against their sources.

**The reader is another agent, not a person.** Write for lookup and trust: dense, literal, dated, no narrative and no presentation. A small topic is one file; for a complex one consider an index file plus a file per thread, so a reader loads only the part it needs. Save where the repo already keeps such notes; with no convention, pick a sensible place and say where.
