---
name: ui-patterns
description: Chooses and builds the right interface pattern for search and autocomplete, empty and no-results states, category and browse screens, numeric and choice inputs, progressive disclosure, tooltips, personalization and recommendations, and order-tracking or post-purchase screens. Use when designing or reviewing any of those surfaces, deciding between two controls, or when a screen works but feels effortful, blank, or noisy.
license: Apache-2.0
---

# UI patterns

Interface quality that is not persuasion. These surfaces rarely appear in conversion advice, but they are where products feel effortless or exhausting: the moment a user taps into search, lands on an empty screen, tries to enter a number, or waits for a delivery.

The shared rule: **meet the user at the moment they are actually in.** A search bar that has just been focused is a moment of intent with no query yet. An empty list is a moment of confusion. A tracking page is a moment of anxiety. Each has a right answer, and it is usually not "show a blank area and wait".

## Choosing a pattern

| You are building | Read |
|---|---|
| Search, autocomplete, no-results, category and browse screens | `references/search-and-browse.md` |
| Numeric input, sliders, pickers, drop-downs, radios, checkboxes, quantity | `references/inputs-and-controls.md` |
| Empty states, accordions, tooltips, modals, error feedback, microcopy | `references/disclosure-and-feedback.md` |
| Home screens that adapt by user stage, recommendations, order tracking, confirmation pages | `references/adaptive-and-post-purchase.md` |

## Non-negotiables

- **Every control has a keyboard path.** Any drag interaction — range sliders are named explicitly in WCAG 2.2 SC 2.5.7 — needs a single-pointer, non-dragging alternative. Sliders need clickable tracks and arrow-key stepping.
- **Targets are at least 24 × 24 CSS px** (WCAG 2.2 SC 2.5.8), and design to platform guidance rather than the floor: Apple 44 × 44 pt, Android 48 dp.
- **Persistent visible labels.** Placeholder text is never the only label.
- **Nothing required to complete a task lives in a tooltip**, a hover, or a third disclosure level.
- **Feedback is never colour alone** — text plus icon plus colour.
- **Never show a "no records" message that later populates.** If data is still loading, say it is loading.

## Grading

Rules are tagged `(measured)` where a controlled study or a benchmark with a stated denominator supports them, `(qualitative)` where the source is moderated usability testing without published sample sizes, and `(untested)` where the pattern is reasonable but nobody has measured it. Do not upgrade a `(qualitative)` rule into a promised outcome.

Baymard and NN/g are the backbone here. Their **site-failure percentages** ("28% of sites use drop-downs") are countable and reliable. Their **user-behaviour percentages** come from moderated testing whose sample size and year are often unstated — cite the mechanism and the rule, not a lift.
