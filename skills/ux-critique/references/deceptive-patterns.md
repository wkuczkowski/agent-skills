# Deceptive patterns and legal checks

Findings here are **P0 or P1 by default** — they are not usability opinions, and they do not trade off against taste.

Use the regulator vocabulary so findings map onto enforcement language. Brignull's taxonomy: *forced action, sneaking, hard to cancel, preselection, obstruction, hidden subscription, hidden costs, trick wording, visual interference, fake social proof, fake urgency, nagging, confirmshaming, fake scarcity, disguised ads, comparison prevention, addictive design, currency confusion.* EDPB's six categories for consent interfaces: *overloading, skipping, stirring, obstructing, fickle, left in the dark.*

> **Scope note.** These are design checks derived from primary legal sources, not legal advice. Report them as "this pattern is named in X and needs legal review", not as a verdict. Status changes: the FTC's click-to-cancel rule was **vacated in full on 8 July 2025** and as of Aug 2026 there is no federal click-to-cancel rule in force — ROSCA and state laws still bind. Do not cite the vacated rule as binding.

## Consent and preselection

| Check | Fails against |
|---|---|
| Any pre-ticked consent, marketing opt-in, cookie choice or data-sharing toggle | GDPR Art. 4(11), Recital 32; CJEU C-673/17 *Planet49* |
| Any pre-ticked box adding a payment beyond the main obligation | CRD 2011/83/EU Art. 22 — consumer is entitled to **reimbursement** |
| Withdrawing consent takes more interactions than granting it | GDPR Art. 7(3) |
| The privacy-protective path takes more steps than the data-invasive one | EDPB Guidelines 03/2022 |
| Cookie banner without a one-click reject at the same level as accept | Colorado Rule 7.09(A)(1); 11 CCR § 7004(a) |
| Accept and decline differ in size, contrast or prominence | Colorado Rule 7.09(A)(1) — greying the decline is named explicitly |
| Consent inferred from silence, scrolling, or closing a pop-up | Colorado Rule 7.09(A)(3) |
| Double negatives or illogical labels ("Yes"/"No" against "provide or decline consent") | Colorado Rule 7.09(A)(7) |

## Pressure and shame

| Check | Fails against |
|---|---|
| Countdown that resets, has no real deadline, or restarts on reload | **UCPD Annex I point 7 — per-se banned, no consumer-harm test**; UK DMCC Sch. 20 para 7; FTC "Baseless Countdown Timer" |
| "Only N left", "limited time", "N people viewing" that cannot be verified against live data | Same, plus FTC "false low stock" |
| Decline labels that shame — "I'll risk it", "No, I like paying full price" | FTC 2022 taxonomy "Confirm Shaming"; Colorado Rule 7.09(A)(2) |
| Re-prompting a user who already declined within the same visit; stacked pop-ups | Colorado Rule 7.09(A)(6); DSA Art. 25(3)(b) |
| Modal that cannot be dismissed without an action, for a non-blocking ask | NN/g: most-hated technique measured; forced dismissal is the discriminating variable |

**Intent is not a defence, and common use is not a defence.** The test is effect (11 CCR § 7004(b)–(c); Colorado Rule 7.09(C)–(F)). "Every competitor does this" is not a rebuttal to any finding on this page.

## Subscriptions

| Check | Fails against |
|---|---|
| Cancelling takes more steps than subscribing, or a different medium | DSA Art. 25(3)(c); Cal. AB 2863; ROSCA § 8403 |
| Charge amount, frequency or cancel-by date not disclosed **before** the billing field | ROSCA 15 U.S.C. § 8403 |
| Consent to the recurring charge is not a separate affirmative act from the purchase | ROSCA |
| A promised trial-end reminder that is not actually implemented | Misrepresentation. Note Apple sends no automatic trial-end reminder — if the screen promises one on iOS, it must be built |
| Renewal terms, duration or cancellation buried or omitted | UCPD Arts. 6–7 (misleading omission); CRD Art. 6(1)(o)–(p) |

## Social proof and reviews

| Check | Fails against |
|---|---|
| Any count, rating or testimonial not traceable to real data | FTC Act § 5 substantiation |
| Rating summary filtered by sentiment while implying it is the full set | 16 CFR § 465.7(b); UK DMCC Sch. 20 para 13 |
| Negative reviews suppressed while positive ones publish | Same |
| Insider or employee testimonials without a material-connection disclosure; AI-generated reviews | 16 CFR §§ 465.2, 465.5 |
| A review property the company owns labelled "independent" | 16 CFR § 465.6 |
| Purchased followers, views or likes displayed | 16 CFR § 465.8 |
| "Best seller" / "Most popular" that cannot be evidenced on demand | FTC Act § 5; UCPD Art. 6 |

## Prices

| Check | Fails against |
|---|---|
| Struck-through "was" price not computed from the **lowest price actually charged in the previous 30 days** | Directive 98/6/EC Art. 6a; CJEU C-330/23 *Aldi Süd* — actively enforced; an EU sweep of 314 traders found ~30% failing |
| A reference price a merchandiser can type in by hand | Same — flag the *architecture*, not just the instance |
| US "former price" that was not offered regularly for a substantial period | 16 CFR 233.1 |
| UK invitation to purchase without total price, delivery charges, taxes and cancellation rights | UK DMCC Act 2024 s.230 — **all categories** |
| Live-event ticket or short-term lodging where the total is not the most prominent price | 16 CFR Part 464 — **this scope is narrow; do not apply it to SaaS or retail** |
| "Free" where the consumer must pay anything beyond responding and delivery | UK DMCC Sch. 20 para 23 |
| Price quoted that changes at checkout | UCPD misleading action |

## How to report one of these

State the pattern by its regulator name, quote the interface evidence, name the instrument, and say what the compliant version looks like. Do not soften it into a suggestion, and do not overstate it into a verdict.

> **[P0] Fake urgency — countdown resets on reload.**
> Evidence: `<div class="countdown" data-start="onload">` — the timer is initialised from page load, not from a fixed end time.
> Cost: the deadline is not real, so the user is pressured by a fiction; and this is a per-se banned commercial practice in the EU (UCPD Annex I point 7) and UK (DMCC Sch. 20 para 7), with no consumer-harm test required. Needs legal review.
> Fix: bind the timer to a real `endsAt` timestamp from the offer record and let it reach zero, or remove it. If the offer has no end date, there is no timer.
