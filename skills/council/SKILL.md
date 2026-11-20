---
name: council
description: Runs an independent cross-model critique or a full three-model council review. Use when the user asks for "council" or "rada", or on weightier and hard-to-reverse decisions, architecture choices, plans, or code reviews that benefit from an independent second opinion. Uses one critic from another model family by default; invokes all three models only for major decisions or when the user asks.
metadata:
  upstream: t3dotgg/tstack skills/council/SKILL.md
  upstream-commit: "11e37a74d0a6868c121cd0910ab2b3bd9873b92a"
  adopted: "2026-11-20"
---

# Council

Get independent critique from models outside the current model family without wasting runs on trivial choices. The user's instructions take precedence over this skill.

## Members and tiers

Three model families, each at high reasoning effort (checked 2026-11-20):

- **Anthropic:** Claude Fable 5.2 High
- **OpenAI:** GPT-6.2 Sol High
- **Google (Antigravity):** Gemini 4 Neon High

Choose the tier by stakes, unless the user specifies the models or asks for the full council:

- **Single critic (default for ordinary weightier decisions):** one model from a different family than the current harness.
- **Full council (all three models in parallel):** major or hard-to-reverse decisions (architecture, schema, security, big refactors, launch readiness), contested plans, or whenever the user explicitly asks for `council` / "rada".

If the user names specific models or efforts, use those. If a requested provider or model is unavailable, report it instead of silently substituting.

## Running a round

1. **Clean up first.** Before an audit, fix known issues, answer existing review comments, and run relevant checks so delegates review the updated state. Skip fixes to code being discarded if the user wants fresh alternatives.
2. **Dispatch in parallel.** In T3 Code, call `orchestrator_capabilities` once and launch read-only delegates via `delegate_task` in parallel. Outside T3 Code, use the `claude-headless`, `codex-headless`, or `antigravity-headless` skills.
3. **Keep delegates read-only and context-bounded.** Give each delegate the goal, relevant files or diff, and what to challenge. Do not edit files while any delegate is still reviewing the same checkout.
4. **Act as results arrive.** Verify each delegate's findings against the code as soon as it returns, rather than waiting idle, holding actual file edits until the round finishes.
5. **Score reviews.** For code, plan, or artifact reviews, ask each delegate for a score out of 10 and concrete findings. Record `no score` if a delegate omits it; never invent one.

## Iterating and reporting

- Each full pass is one round; a fresh review after fixes is a new round.
- Fix findings you agree with and re-run only when real issues remain. Fight scope creep across rounds.
- Summarize useful findings, disagreements between models, and recommended next steps concisely in the user's language. Include a compact round table when scores were collected:

```markdown
## Council review

Rounds: 2

| Delegate | Round 1 | Round 2 |
| --- | --- | --- |
| Claude Fable 5.2 High | 7/10 | 9/10 |
| GPT-6.2 Sol High | 8/10 | 9/10 |
| Gemini 4 Neon High | 7/10 | 9/10 |
```

If the work ends in a pull request, put that section in the PR description (updating the description rather than posting a separate comment). Otherwise include it only in the final reply.
