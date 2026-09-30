# Bright lines — law, deceptive patterns, accessibility

Rules an implementation must satisfy and the checks an audit runs against them. Each legal rule carries a jurisdiction and a status because several changed recently. They are not style preferences, do not yield to a brief, and do not trade off against taste; in an audit a hit here is P0 or P1 by default.

> **Currency and scope.** Two items in this area changed inside the last fourteen months (the FTC click-to-cancel vacatur; the FTC's Feb-2026 restoration of the prior rule). Re-check dates before relying on any of this in a shipped compliance decision. This file is design guidance written from primary sources, not legal advice, and the EU texts below were read from mirrors rather than the Official Journal, so a lawyer confirms article-level wording before it drives a product decision. In an audit, report a hit as "this pattern is named in X and needs legal review", not as a verdict.

> **US federal subscriptions, status as of Aug 2026.** The FTC's 2024 Negative Option "click-to-cancel" Rule was **vacated in its entirety** by the Eighth Circuit on 8 July 2025 (*Custom Communications, Inc. v. FTC*, 142 F.4th 1060) on procedural grounds — the court did not reach the substantive challenges. The FTC restored 16 CFR 425 to its pre-2024 text in Feb 2026 and reopened comment in Mar 2026. **There is currently no federal click-to-cancel rule in force**; do not cite the vacated rule as binding. ROSCA (15 U.S.C. § 8403) and state laws still bind.

**Vocabulary.** Regulator names make findings map onto enforcement language. Brignull's taxonomy: *forced action, sneaking, hard to cancel, preselection, obstruction, hidden subscription, hidden costs, trick wording, visual interference, fake social proof, fake urgency, nagging, confirmshaming, fake scarcity, disguised ads, comparison prevention, addictive design, currency confusion.* EDPB's six categories for consent interfaces: *overloading, skipping, stirring, obstructing, fickle, left in the dark.*

---

## Consent and defaults

| # | Rule | Basis | Where |
|---|---|---|---|
| L1 | No consent checkbox, marketing opt-in, cookie/tracking choice or data-sharing toggle may be pre-ticked or pre-enabled. | GDPR Art. 4(11), Recital 32 ("silence, pre-ticked boxes or inactivity should not constitute consent"); CJEU C-673/17 *Planet49* | EU |
| L2 | Privacy-protective settings must be the default state. | GDPR Art. 25(2) | EU |
| L3 | No pre-ticked box may add any payment beyond the main contractual obligation; if it does, the consumer is **entitled to reimbursement**. | CRD 2011/83/EU Art. 22 | EU |
| L4 | Withdrawing consent must take no more interactions than granting it. | GDPR Art. 7(3) | EU |
| L5 | The privacy-protective path must not require more steps than the data-invasive path. | EDPB Guidelines 03/2022, "Longer than necessary" | EU |
| L6 | A consent request bundled into a wider declaration must be clearly distinguishable, intelligible and in plain language. Burden of proof is on you. | GDPR Art. 7(1)–(2) | EU |
| L7 | Cookie banners must offer accept, reject and manage at the same level — one click each. "Accept All" + "More information" fails; "Accept All" + "Decline All" passes. | Colorado Rule 7.09(A)(1); 11 CCR § 7004(a) | EU / CO / CA |
| L8 | Accept and decline controls must be equal in size, contrast and visual salience. Greying the decline is named explicitly as a violation. | Colorado Rule 7.09(A)(1) | CO |
| L10 | Silence, inaction, closing a pop-up, or scrolling is not consent. | Colorado Rule 7.09(A)(3) | CO |
| L13 | No double negatives or illogical labels. "Yes"/"No" against "Do you wish to provide or decline consent" fails — the labels must be "Provide"/"Decline". | Colorado Rule 7.09(A)(7); 11 CCR § 7004(a) | CO / CA |

## Pressure, shame and nagging

| # | Rule | Basis | Where |
|---|---|---|---|
| L9 | No confirmshaming. Decline labels stay neutral. Fails: "No, I don't want to save money", "I'll risk it", "No, I like paying full price", "No, I don't care about animals". | FTC 2022 dark-patterns taxonomy ("Confirm Shaming"); Colorado Rule 7.09(A)(2) | US / CO |
| L11 | No countdown timer, "only N left", "limited time" or "N people viewing" claim unless the constraint is real, verifiable against live data and does not reset or restart on reload. | FTC 2022 ("Baseless Countdown Timer", "False Limited Time Message", false low stock); **UCPD Annex I point 7 — per-se banned, no consumer-harm test needed**; UK DMCC Sch. 20 para 7 | US / EU / UK / CO |
| L12 | No re-prompting a user who already declined within the same visit; no stacked pop-ups; no redirect away for declining. | Colorado Rule 7.09(A)(6); DSA Art. 25(3)(b); EDPB "Continuous prompting" | CO / EU |
| L28 | **Intent is not a defence.** An interface that has the effect of subverting choice is a dark pattern, and consent obtained through it is invalid. Common use is not a defence either: "every competitor does this" rebuts no finding in this file. | 11 CCR § 7004(b)–(c); Colorado Rule 7.09(C)–(F) | CA / CO |
| N1 | A modal that cannot be dismissed without an action, for a non-blocking ask. Not a statute: modal interruption is the most-hated technique measured, and forced dismissal is the discriminating variable (data in `disclosure-and-feedback.md`). | NN/g research | — |

## Subscriptions and cancellation

| # | Rule | Basis | Where |
|---|---|---|---|
| L14 | Cancelling must be at least as easy as subscribing, in the same medium used to sign up. | DSA Art. 25(3)(c); Cal. B&P § 17600 et seq. as amended by AB 2863; ROSCA § 8403 | EU / CA / US |
| L15 | Material subscription terms — that the user will be charged, the amount, the frequency, the cancel-by date — must be disclosed **before** the billing-information field, and consent to the recurring charge must be a separate affirmative act from the purchase. | ROSCA 15 U.S.C. § 8403 | US (statute in force) |
| L16 | Contract text must not undermine the consumer's ability to give affirmative consent. Retain consent records. | Cal. AB 2863, from 1 July 2025 | CA |
| L31 | Renewal terms, duration or cancellation must not be buried or omitted. | UCPD Arts. 6–7 (misleading action / misleading omission); CRD Art. 6(1)(o)–(p) — see L30 | EU |
| P1 | **Platform rules.** Apple Review Guideline 3.1.2 requires describing what the customer gets for the price before subscribing; trial-specific disclosures sit in Schedule 2 of the Developer Program License Agreement. Apple sends automatic notifications only for price increases and failed renewals — **there is no guaranteed trial-end reminder on iOS, so build your own.** Google emails a reminder before a free trial or intro price ends. A screen that promises a trial-end reminder that is not actually implemented is a misrepresentation. | Apple / Google developer policy | platform |
| P2 | California AB 2863 does **not** mandate reminders for typical app trials: pre-conversion notice is required only where the free period exceeds **31 days**, and renewal reminders only for initial terms of a year or more. A 3- or 7-day trial triggers neither. | Cal. B&P § 17602(b) | CA |

## Reviews, ratings and social proof

| # | Rule | Basis | Where |
|---|---|---|---|
| L17 | Never show a rating summary filtered by sentiment or star rating while implying it is the full set. Never suppress negative reviews while publishing positive ones. | 16 CFR § 465.7(b); UK DMCC Sch. 20 para 13 (per-se banned) | US / UK |
| L18 | Insider, employee and manager testimonials require a clear material-connection disclosure. No fake or AI-generated reviews. | 16 CFR §§ 465.2, 465.5 | US |
| L19 | Never label a review property you own or control as "independent". | 16 CFR § 465.6 | US |
| L20 | Never buy or display fake indicators of social influence (followers, views, likes). | 16 CFR § 465.8 | US |
| L21 | Never use legal threats or intimidation to suppress a review. | 16 CFR § 465.7(a) | US |
| L22 | A "Best seller" / "Most popular" label must be factually true and evidenceable on demand. | FTC Act § 5 substantiation; UCPD Art. 6 | US / EU |
| L32 | Every count, rating or testimonial on screen must be traceable to real data. | FTC Act § 5 substantiation | US |

## Price display

| # | Rule | Basis | Where |
|---|---|---|---|
| L23 | Any struck-through "was" price and any percentage-off must be computed from the **lowest price actually charged in the previous 30 days**. Build the reference price as a data field sourced from real price history — make it structurally impossible for a merchandiser to type one in. In an audit, a reference price that can be typed by hand is a finding about the *architecture*, not just the instance. Actively enforced: an EU price-marketing sweep of 314 traders found ~30% failing. | Directive 98/6/EC Art. 6a (Omnibus, applicable 28 May 2022); CJEU **C-330/23 Aldi Süd**, 26 Sept 2024 | EU |
| L24 | A "former price" must be an actual bona fide price offered to the public on a regular basis for a reasonably substantial period. | 16 CFR 233.1 | US |
| L25 | For **live-event tickets and short-term lodging only**: the total price must be displayed more prominently than any other pricing information, and every excluded fee disclosed before consent. **Do not generalise this scope** — SaaS, retail and delivery are not covered. | 16 CFR Part 464, effective 12 May 2025 | US |
| L26 | For any UK invitation to purchase, in **all** categories: total price or how it is calculated, plus delivery charges, taxes not included, and cancellation rights. Omission includes information given "in a way that is unclear or untimely". | UK DMCC Act 2024 s.230, from 6 Apr 2025 | UK |
| L27 | "Free" may not be claimed where the consumer must pay anything beyond the unavoidable cost of responding and of delivery. | UK DMCC Sch. 20 para 23 | UK |
| L33 | A quoted price must not change at checkout; if price genuinely varies, show one number plus an explicit plain-language condition (`pricing-and-plans.md`). | UCPD misleading action; an unfair commercial practice in the UK and EU | EU / UK |

## Scope note

| # | Rule |
|---|---|
| L29 | DSA Art. 25 binds **providers of online platforms** (intermediaries hosting user content shared with the public), and Art. 25(2) carves out practices already covered by the UCPD or GDPR. It is residual. |
| L30 | For a first-party DTC store the operative EU hooks are **UCPD Arts. 6–7** (misleading action / misleading omission of renewal terms, duration, cancellation) and **CRD Art. 6(1)(o)–(p)** — not DSA Art. 25, and for a main-obligation plan choice not CRD Art. 22 either. |

## What the compliant version looks like

| Pattern | Compliant version |
|---|---|
| Invented social proof — "500+ sold this week", "1,247 people are viewing this", "Join 10,000+ happy customers", a testimonial trio with generated names and stock faces (L20, L22, L32) | Wire the count to a live query, define the window in the label ("sold in the last 7 days"), be able to reproduce the number on demand, and show nothing when the number is unimpressive. In a mockup, mark the slot as data the user must supply — never fill it with a plausible figure. A number the team cannot reproduce on demand is a landmine for whoever inherits the page. |
| Fake urgency (L11) | Real deadlines only, from real data. If the constraint is real, say what happens at the deadline and let the user act after it. A timer binds to a real `endsAt` from the offer record; no end date means no timer. |
| Confirmshaming (L9) | Neutral decline labels ("Not now"), and the decline is remembered for the rest of the visit. |
| Asymmetric choice — bright large "Accept all" beside a greyed small "Manage preferences"; accept in one click, decline in three (L5, L7, L8) | Equal weight, equal step count, both paths visible at the same level. |
| Pre-ticked consent or pre-added charges — a checked newsletter box, a pre-added insurance add-on, a pre-enabled data-sharing toggle (L1, L3) | Default effort, never permission or money. |
| The exit harder than the entrance — subscribe in two taps, cancel by emailing support (L14) | Cancel in the same medium, in no more steps than signup took. |
| Nagging — a dismissed modal re-shown on the next page, stacked pop-ups, an interstitial that requires an action to escape (L12, N1) | One ask, remembered. Inline placement over interruption. |

---

## Accessibility

Failing these is a conformance defect, not a style opinion. In the EU the **European Accessibility Act (Directive 2019/882)** brings e-commerce, consumer banking, e-books, passenger transport and consumer computing devices into scope — private sector, not only public bodies.

| # | Rule | Criterion | Level |
|---|---|---|---|
| A1 | Every input collecting information *about the user* carries the correct `autocomplete` attribute. (The criterion is scoped to user-information fields, and `autocomplete` is a sufficient technique — do not overstate it as "every field".) | WCAG 2.1 SC 1.3.5 Identify Input Purpose | AA |
| A2 | No authentication step requires a cognitive function test — password recall, a puzzle — without an alternative route. **Copy-paste of credentials must not be blocked.** Kills: password-recall-only login, paste-blocked OTP boxes, puzzle CAPTCHAs. | WCAG 2.2 SC 3.3.8 Accessible Authentication | AA |
| A3 | Information already entered in the same process is auto-populated or selectable. Its own example: **do not clear the card number when showing a validation error.** | WCAG 2.2 SC 3.3.7 Redundant Entry | **A** |
| A4 | Targets are at least **24 × 24 CSS px**, or spaced so a 24px circle centred on the target does not intersect another. | WCAG 2.2 SC 2.5.8 Target Size (Minimum) | AA |
| A5 | Any drag interaction — **range sliders are named explicitly** — has a single-pointer, non-dragging alternative. Make tracks clickable; support arrow-key stepping. | WCAG 2.2 SC 2.5.7 Dragging Movements | AA |
| A6 | Sliders expose `role="slider"` with `aria-valuenow` / `-valuemin` / `-valuemax` / `-valuetext` so a screen reader announces a meaningful value, not a bare number. | ARIA practice supporting A5 | — |
| A7 | Errors are conveyed by **text + icon + colour**, never colour alone. | Use of colour | AA |
| A8 | Every field has a **persistent visible label**. Placeholder text is never the only label — it strains memory, prevents pre-submit verification, is mistaken for a pre-filled value, and announces unreliably to screen readers. | NN/g form research | — |
| A9 | Consent mechanisms have **action parity** for assistive tech: if consent takes two clicks with a mouse, it must take no more than two actions with an accessibility tool. | Colorado Rule 7.09(A)(9) | CO |
| A10 | Design to platform targets, not the legal floor: **Apple 44 × 44 pt**, **Android/Material 48 dp**. | Platform guidelines | — |

## Timers and time limits

A countdown that gates a task also engages WCAG time-limit requirements, on top of L11. If the constraint is real, provide a way to extend or a route that does not expire.

---

## Reporting a hit in an audit

State the pattern by its regulator name, quote the interface evidence, name the instrument, and say what the compliant version looks like. It is neither softened into a suggestion nor overstated into a verdict.

> **[P0] Fake urgency — countdown resets on reload.**
> Evidence: `<div class="countdown" data-start="onload">` — the timer is initialised from page load, not from a fixed end time.
> Cost: the deadline is not real, so the user is pressured by a fiction; and this is a per-se banned commercial practice in the EU (UCPD Annex I point 7) and UK (DMCC Sch. 20 para 7), with no consumer-harm test required. Needs legal review.
> Fix: bind the timer to a real `endsAt` timestamp from the offer record and let it reach zero, or remove it. If the offer has no end date, there is no timer.
