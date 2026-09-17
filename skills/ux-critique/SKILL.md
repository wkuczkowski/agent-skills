---
name: ux-critique
description: Audits an existing screen, page, flow or component for usability, conversion and deceptive-pattern problems, and proposes concrete pasteable rewrites. Use when asked to review or critique a design, screenshot, landing page, checkout, paywall or signup flow, to say why something is not converting, or to check an interface for dark patterns and accessibility defects.
license: Apache-2.0
disable-model-invocation: true
---

# UX critique

An audit is worth having only if its findings are real. The failure mode of this task is not missing problems — it is **manufacturing** them, because a review that returns little looks like a review that did little. Everything below exists to make that harder.

## Run it in two stages

**Stage 1 — find.** Sweep the interface against the checks in `references/sweeps.md` for the surfaces present. Be generous here; collect more than you will report. Record evidence as you go, because you cannot reconstruct it later.

**Stage 2 — filter, in a separate pass.** Take each candidate finding and argue *against* it. Would a senior designer actually raise this? Is the evidence in hand, or inferred? Is it in scope? Drop anything that survives only because it sounded plausible.

Where a sub-agent is available, run stage 2 as its own agent given only the finding and the artifact — not your stage-1 reasoning. The finder has an interest in looking useful; the filter must not inherit it. If you cannot isolate the passes, say so in the report: `⚠️ single-pass audit — findings not independently filtered`.

## Before either stage: fix the scope

Write these down first. An audit without them produces arbitrary results.

- **The task scenario.** "A first-time visitor on mobile trying to buy one item as a guest." Not "the checkout".
- **What you can actually observe** — a static screenshot, a live URL, the source, the running app.
- **What you cannot** — hover and focus states, dark mode, logged-in views, error states, real content lengths, other breakpoints, anything behind an interaction you did not trigger.

Evaluator agreement between two people auditing the same system with the same method ranges from **5% to 65%** (Hertzum & Jacobsen 2001), and that holds for severity ratings too. The documented causes are vague task scenarios, vague procedure, and vague problem criteria. Fixing the scope is what makes the output reproducible rather than one sample of an evaluator's opinion.

## Evidence contract

Every reported finding carries five slots. A finding missing any of them is not reportable.

| Slot | Requirement |
|---|---|
| **Severity** | P0–P3, per the scale below |
| **What** | The defect, stated in one sentence |
| **Evidence** | Quoted markup, a computed value, a named screen region, or a specific observed behaviour. Not "the hierarchy feels weak" |
| **Cost** | What it costs the *user* — the action they cannot complete, the question they cannot answer, the mistake they will make. Never a principle name as the cost |
| **Fix** | Pasteable. Real copy, real hex values, real CSS properties, real HTML. "Improve the contrast" is not a fix; `#767676` on `#FFFFFF` is |

Mark every value you did not measure as `(inferred)`. If you are reading a screenshot, you did not measure the contrast ratio — you estimated it. Never state a hex value, a pixel measurement or a ratio you did not read from the artifact.

## Severity

| | Definition |
|---|---|
| **P0** | Blocks the task, loses data, or is unlawful. The user cannot complete what they came to do, or the interface breaks a binding rule |
| **P1** | Causes a substantial number of users to fail, abandon, or make a wrong choice. Recoverable but costly |
| **P2** | Slows or confuses users; adds effort without blocking |
| **P3** | Polish. Correct to fix, not worth a release |

**Tiebreaker between two levels: would a user contact support about this?** If yes, it is at least P1.

Legal and accessibility conformance failures are P0 or P1 by definition, never P3 — they do not compete with usability findings for priority and are not a matter of taste.

## Hard exclusions

Do not report any of the following. This list is the main defence against padded output.

1. Anything a linter, axe, Lighthouse or a typechecker already reports.
2. Pre-existing issues outside the stated scope or on parts of the interface the user did not ask about.
3. Taste preferences with no user-facing cost — font choice, palette, corner radius, spacing you would have done differently.
4. Anything visible only in a state you did not observe. If you did not check the hover state, you have no hover finding.
5. Speculation about performance, analytics or backend behaviour you cannot see.
6. "This could be more delightful", "consider adding personality", "the design could be more modern".
7. A missing feature, unless its absence blocks the stated task.
8. Restating a documented best practice the design already follows.
9. Findings whose only support is a statistic. Much of the folklore in this field is fabricated — "70–90% never change defaults", "trust badges convert 23% better", "progressive disclosure gives 30–50% faster completion" all have traceable laundering paths and no source. The `conversion-ux` skill carries the full drop-list in its evidence reference.
10. Any finding you could not write an evidence line for.
11. Duplicate expressions of one underlying defect. Report the cause once.
12. Anything you would drop if asked "is this actually worth the author's time?"

## Never invent the cure

This is where an audit does real damage. The correct fix for many findings *is* a specific number — a review count, a sold-this-week figure, a delivery date, a rating — and a specific number always reads better than a vague one.

**Propose the slot; never fill it.** Write `[real count from live query]`, not "500+ sold this week". Write `[actual delivery date]`, not "Arrives Thursday". A fabricated specific is worse than the vague copy it replaced, because it will ship.

The same applies to testimonials, customer names, logos, certifications, statistics in marketing copy, and any deadline. If the data does not exist, the finding is "this page has no substantiated social proof and needs one" — not a draft with numbers in it.

## Output

Report in this order, with these budgets:

1. **Scope** — task scenario, what you examined, one line.
2. **Findings** — **maximum 5**, most severe first, each with the five slots. Fewer is a normal result.
3. **What I could not see** — mandatory. List every state, breakpoint and view outside your observation. This section is never empty; if you think it is, you have not thought about hover, focus, dark mode, error states, long content, or small screens.
4. **What works** — 2–3 items, specific. Skip if there is genuinely nothing, and do not invent.

If nothing meets the bar, say exactly that: *"No findings above P2 for this scenario. Checked: [list]."* Then stop.

**"No significant issues found" is a forbidden conclusion.** Absence of a finding is never evidence of absence — it is evidence about your scenario and your coverage. Never claim to have audited a full flow; claim only what the stated scenario touched.

## Reference index

| File | What it answers |
|---|---|
| `references/sweeps.md` | What to check, per surface — the finding pass |
| `references/deceptive-patterns.md` | The dark-pattern and legal checks, with regulator vocabulary |

For design *rules* and the evidence behind them, the `conversion-ux` and `ui-patterns` skills carry them. This skill is the procedure, not the rulebook.
