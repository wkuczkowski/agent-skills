# Behavioral levers

Each lever: **Rule** → **Mechanism** (with its evidence grade) → **Boundary** (when it backfires). The boundary is not a footnote — most of these levers have a documented reversal.

Grades: `(measured)` controlled evidence · `(mechanism)` psychology replicates, this application untested · `(untested)` never measured in this domain · `(legal)` binding.

---

## 1. Defaults

**Rule.** Decide what happens when the user does nothing before you write a single line of copy. Then instrument the change-rate on every defaulted field.

**Mechanism** `(measured)`. Defaults work through effort/inertia, implied endorsement, and reference-point framing. Meta-analysis: pooled d = 0.68, 95% CI [0.53, 0.83], 58 datasets, N = 73,675 — a 27-point absolute increase, which the authors themselves call medium-sized (Jachimowicz et al. 2019, *Behavioural Public Policy* 3(2)). Users infer *"the provider recommends this"* — directly tested, and the inference partly causes the stickiness (McKenzie, Liersch & Finkelstein 2006, *Psych. Science* 17(5)).

**Boundary.** Heterogeneity is enormous: I² = 98%. Of the 58 datasets, 46 were positive, **10 null, 2 significantly negative**. Defaults weaken where the user has a strong pre-existing preference, stakes are high, the choice is deliberate rather than incidental, or the user suspects your motives — consumers who read a default as marketer self-interest discount it and every other default you set (Brown & Krishna 2004, *JCR* 31(3)).

**Rule** `(legal)`. Never default consent, a legal declaration, or an extra charge. Never default a field you will later analyse as if the user chose it (role, industry, satisfaction) — that manufactures data that looks like preference. And check whether your "most common choice" is only common in your largest locale.

**Rule.** A default and a social-proof label are two different levers. If the justification is popularity, say so in the UI ("most people choose X") rather than relying on the default to carry that meaning silently.

**Rule** `(measured)`. On amount-like fields (contribution rates, donation sizes, plan tiers) a default moves the rate and the average in *opposite* directions: a low default raises take-up and lowers the amount (Goswami & Urminsky 2016, *JMR* 53(5)). Decide which one you are optimizing before you pick the number.

---

## 2. Assortment and complexity

**Rule.** Reduce *perceived* complexity — categorisation, filtering, sorting, defaults — before reducing the number of options.

**Mechanism** `(contested)`. Two meta-analyses disagree head-to-head and neither may be presented as settled: Scheibehenne et al. (2010, *JCR* 37(3), 63 conditions, N = 5,036) find a mean effect near zero; Chernev et al. (2015, *JCP* 25(2), 99 observations, N = 7,202) find it significant once moderators are modelled. The usable output is Chernev's four moderators: **choice-set complexity, decision-task difficulty, preference uncertainty, decision goal**. Overload appears when options are hard to tell apart, the user has no pre-existing preference, and no option dominates.

**Boundary.** Cutting options backfires when users have known preferences and you removed what they came for, and when the assortment *is* the value proposition (marketplaces, catalogues, libraries).

**Never cite the jam study as a law.** See `evidence.md` — the famous 3%/30% are conditional on stopping at a tasting booth, the 24-jam cell is four coupon redemptions, and the larger display drew more traffic.

---

## 3. Progress and investment

**Rule** `(measured)`. Prefer **fast-to-slow pacing** in any progress indicator. Villar, Callegaro & Yang (2013, 32 randomized experiments): fast-to-slow reliably reduced drop-off; slow-to-fast *increased* it; constant indicators did nothing overall and increased drop-off where a small incentive was promised.

**Rule** `(mechanism)`. Front-load visible progress and give any head start a real, stated reason ("Account created ✓"). The goal-gradient effect itself is solid — effort rises as perceived distance to the reward shrinks (Kivetz, Urminsky & Zheng 2006, *JMR* 43(1); the paper's strongest control is 42 customers on non-redeemable cards, who *decelerated*).

**Boundary.** Endowed progress has two randomized field experiments behind it — both on **paper stamp cards** (Nunes & Drèze 2006: 34% vs 19% completion over nine months). Nothing published tests it on software onboarding. Effort also **resets after each reward**, so a series of near finish lines beats one distant one. On a short flow, a progress bar is a step count nobody asked for. Progress inflated beyond what you can honor costs you on the second visit.

**Rule** `(measured)`. Effort earns attachment only if the user **finishes** and can see what they made. IKEA effect pooled d ≈ 0.57 (Pelled, Demetriades & Walter, *Psychology & Marketing* 43); builders valued their boxes at $0.78 vs $0.48 for non-builders, but **incomplete builders at $0.59** (Norton, Mochon & Ariely 2012, *JCP* 22(3)).

**Boundary.** The original manipulations were ten-plus minutes of physical labour producing a visible artifact. Picking a language from a dropdown is not that. A long onboarding users abandon mid-way produces zero investment effect *plus* the drop-off.

**Rule.** Any flow that lets users build state before an account **must preserve that state through signup**. Losing it at the wall is worse than never having offered it.

---

## 4. Walls and gating

**Rule** `(measured, mixed)`. Do not put a wall in front of first value. Move it, don't remove it: a soft dismissible prompt first, a hard wall several steps later. Baymard: 18% abandonment on forced account creation. Duolingo's documented flow puts "Create profile" at ~3:21, after language, goals and lesson content.

**Boundary.** Delaying the wall inflates top-of-funnel and dilutes it — so instrument activation and Day-30/60 revenue per install, not signup count. If your economics need qualified leads, a lower and higher-intent signup rate can be strictly better. The one peer-reviewed study available found free-trial-acquired customers had **59% lower CLV** than directly acquired ones (Datta, Foubert & Van Heerde 2015, *JMR* 52(2)).

**There is no published benchmark for reverse-trial paid conversion.** Free trial and freemium both sit near an 8% median in the best available survey, and that gap disappears once signup rates are accounted for. Anyone quoting reverse-trial conversion rates is quoting nothing.

---

## 5. Social proof

**Rule** `(measured)`. Popularity counts work when they are real. Labelling the five most popular dishes raised demand for them **13–20%** in a randomized field experiment in Chinese restaurants (Cai, Chen & Fang 2009, *AER* 99(3)). Download counts alone causally reshuffled which songs succeeded (Salganik, Dodds & Watts 2006, *Science* 311).

**Implementation** `(legal)`. Wire the count to a live query, define the window in the label ("sold in the last 7 days"), and be able to reproduce the number on demand. A "Best seller" or "Most popular" label is a factual claim requiring substantiation under FTC Act §5 and UCPD Art. 6.

**Boundary.** Popularity cues suppress consideration of alternatives — helpful on a single-product page, harmful on a comparison page where they can push users toward a worse-fitting option. They backfire for uniqueness-seeking buyers and for gift and luxury purchases. Low-volume products should show **nothing** rather than a technically true but unimpressive number.

**Rule** `(measured)`. Show the real rating and real review count, unrounded, and do not chase 5.0 — purchase likelihood peaks around **4.0–4.7** and declines toward 5.0 on a "too good to be true" reading (Maslowska, Malthouse & Bernritter 2017, *IJA* 36(1)). Volume dominates the last decimal: five reviews carry a 270% greater purchase likelihood than none.

**Boundary.** That curve is observational, confounded by review count, and comes from one research group. Treat it as a reason not to chase perfection, not a reason to suppress praise.

---

## 6. Framing and copy

**Rule** `(mechanism)`. Treat loss framing as a variant to test, never as a fixed multiplier. Where you use it, use **specificity** — the actual files, the actual date — as honest disclosure of a real consequence.

**Mechanism.** Concreteness and consequence salience. **Not** a 2× loss coefficient: λ = 2.25 comes from hypothetical lotteries with 25 graduate students; a 607-estimate meta-analysis puts the mean at 1.955, and an 84-paper reanalysis (n = 149,218) puts it at **≈1.07, not significantly above 1** once gains and losses are symmetric (Yechiam & Zeif 2025, *J. Econ. Psych.* 107).

**Do not say "status quo bias makes the threat screen win".** Status quo bias predicts the user taps "later" and keeps their current plan — it works *against* the prompt. The levers are the default and the friction on each path.

**Boundary.** Goal framing is the weakest and least consistent of the three framing types. Loss framing tends to lose to gain framing for prevention and aspirational behaviours, and it raises anxiety and reactance — which can depress trust and retention even where it lifts the immediate click.

**Rule** `(mechanism)`. Acknowledging a small, genuine downside **after** you have made your positive case raises credibility. This is the **pratfall / blemishing effect** and two-sided messaging — there is no such thing as "transparency bias".

**Boundary.** It reverses when the flaw is material, when it leads the message, or when the source is not already perceived as competent. Note that "we'll remind you before we charge" is not a downside disclosure at all; it is reassurance about a known risk.

**Rule** `(measured)`. Make CTA and link labels **Specific, Sincere, Substantial, Succinct** — in that priority order. Specificity beats brevity when they conflict, because eyetracking shows users read links without the surrounding text, so a label must work in isolation. "Learn more" is the new "Click here".

**Boundary.** Clarify ambiguous actions ("Continue to payment", not "Continue"); do not decorate an already-clear one. Keep the primary purchase button literal and conventional — "Add to cart" is the most-scanned element on a product page and users pattern-match on the familiar word. Put brand voice in the microcopy beneath it.

**Specificity in claims** `(mechanism)`. Precise figures read as measured rather than estimated, and anchor more strongly — but only when attributed to a human communicator with conversational intent, and only among consumers low in advertising skepticism (Zhang & Schwarz 2012/2013; Xie & Kronrod 2012). Round numbers work better for feelings-driven purchases. The conversion half of "specificity is trust" has no source.

---

## 7. Pricing presentation

**Rule.** Prefer a single committed price over a range — ranges add uncertainty and make comparison harder, and a range typically produces lower valuations than a single price (Tanford, Choi & Joe 2019).

**Boundary.** Do not commit to a number you cannot honour; a quoted price that changes at checkout is worse than an honest range and can be an unfair commercial practice in the UK and EU. If price genuinely varies, show one number plus an explicit plain-language condition — not a silent range.

**Rule** `(measured)`. Charm pricing only pays at a **left-digit boundary**. Nine-endings are perceived as smaller "only when the leftmost digits differ" — $2.99 vs $3.00 works, $3.29 → $3.19 buys nothing (Thomas & Morwitz 2005). Pick the price, check whether one unit down crosses a leading digit, take it if it does, and otherwise do not contort the number.

**Rule** `(mechanism)`. If you add a high tier, justify it as serving a real segment *and* as a reference point, then measure whether it moves mix. The mechanism is **extremeness aversion / the compromise effect** (Simonson & Tversky 1992) — intermediate options gain appeal — not the attraction effect.

**Boundary.** Do not build an attraction-effect decoy. With images, brands and more than two comparison dimensions — i.e. every real pricing page — an asymmetrically dominated decoy can pull share *away* from its target.

**Anchoring** `(mechanism)`. Anchoring on numeric judgments is among the best-replicated effects in psychology (Many Labs 1). Anchoring on **willingness to pay** is much shakier: anchors moved hypothetical WTP but had no effect on valuations in real, incentive-compatible transactions (Brzozowicz & Krawczyk 2022, N = 1,803). Relative framing ("just 2.6% of your purchase") rests on proportional thinking, which is established — but happens spontaneously in the head, and no study tests the explicit badge.

**Any struck-through comparator is regulated.** See `bright-lines.md` L23–L24.

---

## 8. Controls and inputs

**Rule** `(measured)`. Match the control to whether the user is **exploring** a value or **stating** one they already know. Where both matter, link a slider to a text field showing the same value. Never ship a slider as the only way to set a number.

**Boundary.** Baymard: **83% of top-50 sites using sliders apply a linear scale to non-linearly distributed data**, so half the track controls 2–10% of outcomes; >50% of test subjects misread dual-handle sliders as single-point controls. Their position: if you cannot implement non-linear scaling, distinct handles, click-to-position and a text fallback, do not use a slider at all. Sliders are also named explicitly in WCAG 2.2 SC 2.5.7 — see `bright-lines.md` A5.

**Rule** `(measured)`. Drop-down thresholds: **under ~5 options → radio buttons or a segmented control; 5–10 → drop-down acceptable; over ~10 → autocomplete, lookup, or auto-detect.** 55% of users open a drop-down purely to see what is inside and immediately close it.

**Boundary.** Do not conclude "drop-downs are lazy". They remain correct when the user does not know the option set and cannot type it, and native selects beat custom ones — 31% of sites with custom drop-downs have usability defects in them. Country and state selectors are drop-downs on purpose.

**Rule.** Pre-select one radio button in a group by default — a radio cannot be deselected once clicked, so an all-unselected group traps users who change their mind. Exceptions: you genuinely do not know the preference, the pre-selection risks offending (gender, title), or law forbids it.
