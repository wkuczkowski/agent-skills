# Evidence discipline

Two jobs: stop you repeating fabricated statistics, and tell you when "just A/B test it" is real advice.

## Never repeat these

Each of these circulates widely, sounds authoritative, and is false or unsourced. If you are about to write one, write the replacement instead.

| Never say | Why | Say this |
|---|---|---|
| "24 jams → 3%, 6 jams → 30%" as a law of choice | Those are conversion rates **among people who stopped at a tasting booth**; the 24-jam cell is **four coupon redemptions**; unconditionally it is 1.7% vs 11.9%; **the bigger display drew more traffic**; direct replications failed | "Fewer options helps under specific conditions — hard-to-distinguish options, no pre-existing preference, no dominant choice. Two meta-analyses disagree on the main effect. Test it on your own funnel." |
| "70–90% of users never change defaults" | No source. Traces to a 2011 blog post about mailed-in Word settings files | "Defaults are a large, well-measured effect (pooled d ≈ 0.68, N ≈ 74,000) with very high heterogeneity. In auto-enrolled 401(k)s roughly 75–80% stayed on the default. Measure yours." |
| "Free samples lift purchases up to 2,000%" | Vendor marketing chain; no upstream figure, no method, no denominator | "Sampling produces incremental sales still detectable 12 months later (Bawa & Shoemaker 2004). Never port a retail same-day lift onto a digital funnel." |
| "Cialdini ranked reciprocity as the most powerful driver of human behavior" | He never published a ranking; the principles are scoped to compliance situations | "Reciprocity is one of the principles Cialdini catalogued, and it has the best field evidence of the set (Falk 2007: a small enclosed gift raised donation frequency 17%, a large gift 75%)." |
| "Kahneman won a Nobel for proving losses hurt twice as much" | Wrong prize scope — the 2002 citation names judgment under uncertainty, not loss aversion; and prospect theory is a descriptive model, not a proof | "Prospect theory is the source of loss aversion. Cite the model, not the medal." |
| "The threat screen wins because of status quo bias" | Status quo bias predicts the **opposite** — the user keeps what they have and ignores you | "Status quo bias is why users ignore your prompt. The levers are the default and the friction on each path." |
| "Transparency bias" | **The construct does not exist** in psychology or marketing | "Two-sided messaging / the pratfall or blemishing effect: a small genuine downside disclosed after your positive case raises credibility. Small effect, heavily moderated." |
| "The imagination gap" | Not a name in the literature | "Mental simulation / mental imagery — Elder & Krishna's visual depiction effect; Peck & Shu on imagined touch and psychological ownership." |
| "'My' beats 'your'; 'Start' beats 'Subscribe'; add aspirational copy to the CTA" | One unpublished 2013 landing-page test whose own author reported it failed in Danish; never replicated; academic pronoun work favours **second** person | "Keep the primary purchase button literal and conventional. Put brand voice in the microcopy beneath it. Any copy variant is a hypothesis — and only worth testing if you can reach the sample size below." |
| "1–3 trust signals convert 23% better, 7+ convert 8% worse (Baymard)" | Appears in no Baymard material. One of the most-laundered fake statistics in this field | "Replace decorative badges with verifiable answers to this buyer's actual objection: the return window and who pays return shipping, the delivery date, a link to the real lab report." |
| "Progressive disclosure gives 30–50% faster completion" | Traces to AI-generated blog posts. Nielsen's canonical article contains **no measured effect at all** | "Progressive disclosure is a heuristic with thin backing. Apply Nielsen's own conditions: expose anything users frequently need, and **never exceed two levels**." |
| "This redesign took conversion from 2% to 7%" | No traceable source. Baymard's measured ceiling for a comprehensive checkout redesign on a large site is a **35% relative** increase, needing an average of 32 discrete improvements | Quote the 35% ceiling and the 32 improvements. |
| "Reverse trials convert at 4–6% vs 8–12% for free trials" | These numbers are not on the page they are attributed to | "There is no published benchmark for reverse-trial paid conversion at all." |
| Any "N% lift from one word" | See the arithmetic below — a 90% lift from a one-word change is the statistical signature of an underpowered test | Nothing. Drop it. |

**Rule.** Before repeating any UX or conversion statistic, ask where the number came from. If the chain ends at a vendor blog, an agency listicle or a conference anecdote, it is not evidence. In this corpus, four separate widely-cited figures turned out not to exist on the pages they were attributed to.

## When a test can settle it

"Test it on your own funnel" is unusable unless costed.

| # | Rule |
|---|---|
| M1 | Compute the required sample **first**: `n = 16σ²/δ²` per variant for 80% power at α = 0.05. Worked at paywall/PDP scale — baseline 3.7% conversion, detecting a **10% relative** change → **41,642 users per variant**. If you cannot reach that, do not run the test. Decide on principle, ethics and law instead. |
| M2 | Underpowered tests are worse than no test. Below 0.1 power the probability of getting the **sign** wrong approaches 50%, and low power inflates the magnitude of anything that does reach significance. |
| M3 | Even at p < 0.05, expect a large share of "wins" to be noise. At 80% power the false-positive risk is ~6% for an org with a 33% success rate, but **22% for one with a 10% success rate** — which is most orgs. |
| M4 | Do not peek. Real-time results plus stop-when-significant badly inflates type-I error; per-variant outlier removal can push false positives to 43%. |
| M5 | Twyman's law: any figure that looks interesting is usually wrong. Across tens of thousands of tests at Airbnb, Booking, Amazon and Microsoft, the authors report never seeing a change that improves conversion anywhere near 300%. |

Sources: Kohavi, Deng & Vermeer 2022 (KDD); Johari et al. 2017.

## Open questions — say "untested", not "best practice"

- Does pre-selecting a value in a choice field raise completion? No controlled experiment exists; the Chrome autofill telemetry is explicitly correlational.
- Does a non-zero starting progress bar raise **software onboarding** completion? Two randomized field experiments support the mechanism — both on paper stamp cards.
- Does a trial timeline on a paywall raise conversion, or only reduce refunds and chargebacks?
- Does a lifestyle hero image beat a white-background hero on conversion? Only the image-*set* requirements are evidenced.
- Do specific trust badges out-convert generic ones?
- Does an explicit "just 2.6% of your purchase" badge amplify proportional thinking, or trigger reactance?
- Does the "expensive main product, then cheap add-on below" sequence lift attach rate?

## The uncomfortable one: transparent pricing may cost you money

The only large randomized field experiment in this corpus found the **opposite** of the usual advice: on StubHub, moving fees to the back end raised transactions **14.1%** and revenue **19.5%** (Blake, Moshary, Sweeney & Tadelis 2021, *Marketing Science* 40(4)). The UX counterargument is also weaker than usually stated — Baymard's 40% "extra costs too high" measures *magnitude*, while the item that actually measures surprise is **12%**.

The honest position: **show the all-in total early, and justify it as compliance and trust, expecting a possible short-run revenue cost.** Do not tell anyone that transparent pricing converts better. Note that the StubHub result is a marketplace asymmetry — you lose by showing all-in while competitors do not — which disappears once a category is regulated into all-in pricing, as US live-event ticketing and short-term lodging and all UK B2C invitations to purchase now are.

## Source quality

Lean hardest on **statutes, regulations and court opinions** — they say what they say, and the only risk is currency. Nearly as hard on **large meta-analyses and pre-registered replications**, while quoting their heterogeneity and not only their pooled estimate. Treat **individual classic experiments** as real but singular: cite the design and the exact numbers, and never generalise past the manipulation actually run.

Treat **Baymard and NN/g** as high-quality *qualitative* research with large benchmark denominators and almost no causal claims. Their site-failure percentages ("28% of sites use drop-downs") are countable and reliable; their user-behaviour percentages come from moderated testing whose N and year are often unstated. Cite their mechanism and rule, not a lift.

Treat **vendor benchmark reports** as descriptive, observational, self-selected and commercially motivated — name the denominator every time. Treat **agency blogs and "X% lift from one word"** as worthless as evidence and useful only as a record of what people believe.
