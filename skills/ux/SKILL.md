---
name: ux
description: Designs and reviews user-facing screens (pricing, checkout, paywalls and trials, product pages, signup and onboarding, landing pages, search, forms, empty states, internal app screens) with evidence-graded rules, Baymard and NN/g findings and the legal lines on deceptive patterns. Applies when the user asks to build, change or audit such a screen.
license: Apache-2.0
disable-model-invocation: true
---

# UX

Every element on a screen asks the user a question, and the question decides whether they act or hesitate. A screen that "looks fine" usually fails because it asks a hard question (*Is this worth $19/month? What will this cost in total? What am I locked into?*) where an easy one was available, leaves a real objection unanswered, or ignores the moment the user is actually in: a focused search bar is intent with no query yet, an empty list is confusion, a tracking page is anxiety.

The user's own instructions take precedence over this skill, except where `references/bright-lines.md` marks something unlawful; that conflict goes back to the user.

## What to read

Only the rows that match the task. Every build also finishes against `anti-patterns.md` and the tables in `bright-lines.md` that touch the screen.

| Page, screen or task | Read in `references/` |
|---|---|
| Pricing page, plan comparison, discounts | `pricing-and-plans.md`, `principles.md` |
| Cart and checkout | `checkout.md`, `forms.md` |
| Paywall, free trial, upgrade or win-back prompt | `paywalls-and-trials.md`, `principles.md` |
| Product detail page | `product-pages.md`, `principles.md` |
| Signup and onboarding | `principles.md` (§3 progress, §4 walls), `forms.md`, `adaptive-and-post-purchase.md` |
| Marketing landing page | `principles.md`, `pricing-and-plans.md` ("Trust on a pricing page"), `disclosure-and-feedback.md` (microcopy, modals) |
| Search, autocomplete, no results, category and browse | `search-and-browse.md` |
| Forms and data entry | `forms.md`, `inputs-and-controls.md` |
| Numeric input, sliders, pickers, drop-downs, radios, checkboxes | `inputs-and-controls.md` |
| Empty and error states, tooltips, accordions, modals, microcopy | `disclosure-and-feedback.md` |
| Internal app screens, dashboards, home screens | `disclosure-and-feedback.md`, `inputs-and-controls.md`, `forms.md`, `adaptive-and-post-purchase.md` (stage adaptation) |
| Recommendations and personalization | `adaptive-and-post-purchase.md` |
| Order tracking, confirmation page, post-purchase | `adaptive-and-post-purchase.md` |
| Review or audit of an existing screen | `review.md`, `sweeps.md`, `bright-lines.md`, then the build file for each surface in scope when a finding needs its evidence |
| Any statistic about to be quoted, or "just A/B test it" | `evidence.md` |

## Building or changing a screen

This order is the reverse of how conversion advice is usually written, and the ordering is the point.

1. **Name the decision.** One sentence: what is the user deciding or doing on this screen, and what would make them say no or give up?
2. **Clear the documented blockers.** The largest measured losses are friction, not weak persuasion (the abandonment table in `checkout.md`). These have real numbers behind them; the persuasion levers mostly do not.
3. **Answer the objections on the screen.** Total price, delivery date, return policy, cancellation terms, what happens after the trial, what this empty screen is for. An answered objection beats a persuasive headline.
4. **Then choose behavioural levers** from `principles.md`, each with its boundary condition.
5. **Check `bright-lines.md`.** Several standard conversion tricks are unlawful in the EU, UK or specific US states, and the accessibility rows are conformance, not taste.
6. **Check `anti-patterns.md`.** The generic AI conversion page described there is the default output to avoid.
7. **State what could not be known.** Name every choice that is a hypothesis rather than a rule and what would settle it; `evidence.md` says when a test can.

Rules carry evidence grades defined in `evidence.md`; the grade sets how strongly a rule may be stated, and much of the folklore in this field is fabricated.

## Reviewing an existing screen

Scope first (task scenario, what is observable, what is not), then a generous find pass with `sweeps.md`, then a separate filter pass that argues against each finding, ideally in its own sub-agent. At most five findings, each with severity, what, evidence, cost and a pasteable fix; a mandatory "what I could not see". `review.md` carries the full procedure, the severity scale, the hard exclusions and the output format.

## Non-negotiables

These hold in builds and in the fixes a review proposes, whatever the brief says; the legal basis for each is in `bright-lines.md`.

- No default on a consent, a legal declaration or an extra charge. Default effort, never permission or money.
- No fabricated number: review counts, sales counts, stock levels, deadlines, "N people viewing", testimonials. Where a pattern needs a number that is not available, the slot is marked as data the user must supply.
- No urgency or scarcity that is not real.
- Decline labels stay neutral ("Not now").
- Leaving is no harder than arriving: cancelling takes no more steps than subscribing, withdrawing consent no more than giving it.
- Nothing needed to complete the task lives in a tooltip, a hover or a third disclosure level.
- Every control has a keyboard path, and every drag interaction a single-pointer alternative.
- Targets are at least 24 × 24 CSS px, designed to platform guidance (Apple 44 pt, Android 48 dp).
- Persistent visible labels; placeholder text is never the only label.
- Feedback is never colour alone.
- No "no records" message that later populates; loading data says it is loading.
