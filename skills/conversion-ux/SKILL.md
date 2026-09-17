---
name: conversion-ux
description: Designs and improves conversion-critical screens — paywalls, free trials, pricing pages, product pages, carts, checkout, signup, onboarding and upgrade prompts — using evidence-graded behavioral levers, Baymard/NN-g findings, and the legal bright lines on deceptive patterns. Use when building or changing any screen where a user decides to pay, subscribe, book, sign up, or add to cart, or when a flow "looks fine" but does not convert.
license: Apache-2.0
disable-model-invocation: true
---

# Conversion UX

Every element on a screen asks the user a question. The question determines whether they act or hesitate. A screen that "looks fine" usually fails because it asks a hard question (*Is this worth $19/month? What will this cost in total? What am I locked into?*) where an easy one was available, or because it leaves a real objection unanswered and lets doubt do the deciding.

## Remove friction before you reach for persuasion

Work in this order. It is deliberately the reverse of how conversion advice is usually written, and the ordering is the point.

1. **Name the decision.** Write one sentence: what is the user deciding on this screen, and what would make them say no? Everything below serves that sentence.
2. **Clear the documented blockers.** The largest measured losses are friction, not weak persuasion — undisclosed costs (40% of cart abandonment), slow or unstated delivery (20%), distrust of the payment form (19%), forced account creation (18%), an over-long form (17%). Fix these first; they have real numbers behind them and the persuasion levers mostly do not.
3. **Answer the objections on the screen.** Return policy, total price, delivery date, cancellation terms, what happens after the trial. An answered objection beats a persuasive headline.
4. **Then choose behavioral levers** from `references/principles.md`, each with its boundary condition.
5. **Check the bright lines** in `references/bright-lines.md`. Several standard "conversion tricks" are unlawful in the EU, UK, or specific US states. This check is not optional and not a matter of taste.
6. **State what you could not know.** Name every choice that is a hypothesis rather than a rule, and say what would settle it. See `references/evidence.md` for when a test is even possible.

## Non-negotiables

These hold regardless of what the brief asks for.

- **Never default a consent, a legal declaration, or an extra charge.** Default effort, never permission or money. Pre-ticked consent is invalid under GDPR; a pre-ticked extra payment entitles the consumer to a refund under CRD Art. 22.
- **Never fabricate a number.** No invented review counts, sales counts, stock levels, deadlines, or "X people are viewing this". If a pattern needs a number you do not have, specify the slot and mark it as data the user must supply. A fabricated specific is worse than the vague copy it replaced.
- **Never build urgency or scarcity that is not real.** A countdown that resets, a "limited time" that isn't, a low-stock claim you cannot verify — these are per-se banned commercial practices in the EU and UK, not aggressive marketing.
- **Never shame the decline.** Dismiss labels stay neutral: "Not now", not "No, I like paying full price" or "I'll risk it".
- **Never make leaving harder than arriving.** Cancelling must take no more steps than subscribing; withdrawing consent no more than giving it.
- **Never hide something the user needs to complete the task** inside a tooltip, a hover, or a third disclosure level.

## Evidence discipline

This skill's rules are graded, and the grade changes what you may claim.

| Tag | Meaning | How to speak about it |
|---|---|---|
| `(measured)` | Controlled experiment or large benchmark with a stated denominator | State the rule and the number |
| `(mechanism)` | The psychology replicates; this specific application does not have direct evidence | State the rule, name it as a design bet |
| `(untested)` | Plausible, widely repeated, never measured in this domain | Propose it as a hypothesis, never as a best practice |
| `(legal)` | Statute, regulation, or court decision | Non-negotiable; carries jurisdiction and status |

Do not launder a `(mechanism)` or `(untested)` rule into a promise. "This will lift conversion" is a claim you almost never have standing to make — see `references/evidence.md`, which also carries the sample-size arithmetic that decides whether "just A/B test it" is advice or noise.

Much of the folklore in this area is fabricated. Before repeating any statistic about UX or conversion — including ones that sound authoritative and cite a famous study — check it against the drop-list in `references/evidence.md`.

## Reference index

Read the file for the surface you are working on. Do not read all of them.

| File | What it answers |
|---|---|
| `references/principles.md` | Which behavioral lever applies, its mechanism, and when it backfires |
| `references/paywalls-and-trials.md` | Paywalls, free trials, subscription onboarding, upgrade prompts |
| `references/product-pages.md` | Product detail pages, variant selection, imagery, social proof, badges |
| `references/checkout-and-forms.md` | Cart, checkout, any form: field count, validation, errors, guest flow |
| `references/pricing-and-plans.md` | Pricing pages, plan comparison, price presentation, anchoring, tiers |
| `references/bright-lines.md` | Legal and accessibility rules an implementation must satisfy |
| `references/anti-patterns.md` | The catalog of what goes wrong. **If your design matches an entry, it is wrong.** |
| `references/evidence.md` | Claims to never repeat, and the arithmetic of whether a test can settle a question |

Always finish against `references/anti-patterns.md`.

## Calibration

Conversion screens that AI produces converge on a recognizable default: a three-column pricing table with "Most popular" on the middle tier, green-check feature bullets, a gradient hero above the paywall, a testimonial trio with invented names and photos, "Join 10,000+ happy customers" with no source, trust badges as generic grey icons, and a countdown that starts when the page loads.

Treat that composition as already spent. Any element of it must be earned by the brief and backed by real data, or replaced with the thing the user actually needs to see at that moment — usually the total price, the delivery date, the cancellation terms, or what happens next.
