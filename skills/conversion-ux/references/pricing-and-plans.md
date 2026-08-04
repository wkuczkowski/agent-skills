# Pricing pages and plan comparison

## Show the price

Withholding price is a top abandonment cause. In NN/g's B2B research pricing was the **#1 information need**; participants abandoned sites lacking prices and went to competitors, and companies concealing costs were perceived as "evasive and untrustworthy". `(measured, qualitative)`

When exact pricing is genuinely impossible, use this ladder: **sample prices for representative scenarios → price ranges → MSRP.**

**Avoid interactive pricing calculators.** In testing they were "complex, time-consuming, and error prone", and a simple table of common scenarios outperformed a multi-input calculator.

## One number, not a range

Prefer a single committed price. Ranges add uncertainty and make comparison harder, and a range typically produces *lower* valuations than a single price (Tanford, Choi & Joe 2019). `(mechanism)`

The popular explanation — "the brain grabs the high number" — is shakier than usually stated. The winning account in the anchoring literature has recipients weighting **both** endpoints, and the same literature finds that bolstering range offers get the *offer-maker* better terms in negotiation. The practical case for a single number is **evaluative ease**, not upper-bound anchoring.

**Do not commit to a number you cannot honour.** A quoted price that changes at checkout is worse than an honest range, and can be an unfair commercial practice in the UK and EU. If price genuinely varies, show one number plus an explicit plain-language condition — never a silent range.

## Comparison tables

`(measured)` NN/g's rules, and the failure mode they name first is content, not design:

- **Maximum 5 items.** Compensatory comparison only happens with relatively few alternatives; mobile may support only 2 side by side.
- **Every attribute must have a value for every plan.** A blank cell is read as unknown, not as absent.
- **Consistent terminology.** No playful synonym variation between rows — users read it as a different thing.
- Plans as columns, attributes as rows. Short fragments, not sentences.
- **Sticky column headers** — short-term memory is limited and users forget which column is which.
- Row borders or shading; only attributes users care about; unfamiliar terms defined in context.
- Let users **hide rows identical across all plans**.
- On mobile, convert to tabs or collapsible rows.

**State the difference explicitly.** If plans differ on only a few attributes, promote those to the top — otherwise users assume the options are equivalent. Use language that states the concrete implication, not the category label.

## Tiers and the high anchor

If you add a premium tier, justify it as serving a real segment **and** as a reference point, then measure whether it moves mix. `(mechanism)`

The mechanism is **extremeness aversion / the compromise effect** (Simonson & Tversky 1992) — intermediate options gain appeal, extremes lose it. It is **not** the attraction effect, and the $90-steak-makes-the-$40-salmon-look-reasonable example is a compromise effect, not a decoy: a $90 steak is not dominated by a $40 salmon on any attribute.

**Do not build an attraction-effect decoy.** The attraction effect has a documented replication problem — 91 attempts across 23 product classes produced only 11 reliable effects — and with images, brands and more than two comparison dimensions (i.e. every real pricing page) a dominated decoy can pull share *away* from its target.

## Number formatting

**Charm pricing only pays at a left-digit boundary.** Nine-endings read as smaller "only when the leftmost digits differ": $2.99 vs $3.00 works; $3.29 → $3.19 buys nothing (Thomas & Morwitz 2005). Pick the price, check whether one unit down crosses a leading digit, take it if it does, otherwise leave the number alone. `(measured)`

Round numbers suit hedonic, feelings-driven purchases; precise numbers suit utilitarian ones.

## Discounts and reference prices

A struck-through reference price with a percentage badge reliably raises perceived value and lowers search intention in lab studies — including, notoriously, when the reference price is **exaggerated**, which is exactly why regulators intervened. `(measured in lab, legally constrained)`

`(legal)` **In the EU, any struck-through "was" price and any percentage-off must be computed from the lowest price actually charged in the previous 30 days** (Omnibus Directive Art. 6a; CJEU C-330/23 *Aldi Süd*). The implementation requirement follows: build the reference price as a **data field sourced from real price history**, and make it structurally impossible for a merchandiser to type one in. In the US a "former price" must be a bona fide price offered regularly for a reasonably substantial period (16 CFR 233.1).

An EU price-marketing sweep of 314 traders found roughly **30% failed** discount-display rules. This is actively enforced.

## Add-ons and relative framing

Presenting an add-on next to a much larger purchase, expressed as a share of it ("just 2.6%"), rests on proportional thinking — which is established (people will drive 20 minutes to save $5 on a $15 item but not on a $125 one) but happens **spontaneously in the head**. No published study tests the explicit percentage badge, and it may equally trigger reactance. `(untested)`

Anchoring on willingness to pay is much weaker than anchoring on numeric judgments: anchors moved hypothetical WTP but had **no effect on valuations in real, incentive-compatible transactions** (Brzozowicz & Krawczyk 2022, N = 1,803).

Treat the whole "control the first number they see" family as a hypothesis worth testing, not a rule — and read `evidence.md` first to check whether you can reach the sample size.

## Trust on a pricing page

Four credibility factors, of which two are directly conversion-relevant: `(measured, qualitative)`

- **Upfront disclosure** — prominent contact information, complete pricing with all fees, clear return policies and guarantees, and **no login wall before value**.
- **Connection to the rest of the web** — link out to external review sites and social profiles; people trust external sources more than company-sponsored content.
- Design quality matters mechanically: typos and broken links degrade credibility quickly.

## Checklist

- [ ] Price is visible without contacting sales, or the fallback ladder is used
- [ ] One number per plan, no silent ranges
- [ ] At most 5 plans compared; every cell filled; headers stick; terminology consistent
- [ ] The actual differences between plans are stated, not left to be inferred
- [ ] Any premium tier serves a real segment
- [ ] Every struck-through price is computed from real price history
- [ ] All recurring terms disclosed before the billing step
