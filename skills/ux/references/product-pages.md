# Product detail pages

The measured wins on a PDP are almost all **information the user is hunting for and cannot find**, not persuasion. Baymard's benchmark finds 52% of desktop, 62% of mobile and 64% of app product pages rate "mediocre or worse" — so do not copy a competitor's page. Check against guidelines, not observed practice.

## Answer the hunt first

| Rule | Number | `grade` |
|---|---|---|
| Show estimated **shipping cost on the PDP** | 64% of users look for it there; **43% of sites don't provide it**. Hesitation scales with the *ratio*: $4.97 shipping on a $4.34 item stalls users; $10 on a $400 camera does not | `(measured)` |
| Put "free shipping" **in the buy section**, not only in a site-wide banner | 32% get this wrong | `(measured)` |
| Give on-page access to the **return policy** | 60% of users look for it on the PDP; **44% of sites give no on-page access**; 15% abandoned last quarter over an unsatisfactory return policy | `(measured)` |
| Re-sync **all** data on variant change — gallery, thumbnails, price, discount, stock, size/fit, shipping, included accessories, variant-scoped reviews | 28% of sites don't | `(measured)` |

## Imagery: ship a set, not a better hero

The advice "show the product in use, not isolated on white" has a real mechanism — **mental simulation / the visual depiction effect** (Elder & Krishna 2012), and imagined touch drives psychological ownership (Peck & Shu 2009). But the specific lifestyle-hero-beats-white-hero A/B claim is untested. `(mechanism)`

What *is* measured is the **image set**: `(measured)`

- Plain cutout, in-scale shot against a familiar reference, feature close-ups, human model where applicable, included-accessories shot, zoomable resolution, thumbnail strip.
- **42% of users judge physical size from images alone**; 28–37% of sites have no in-scale image.
- 25% lack sufficient resolution or zoom; 23% omit human-model shots for apparel, accessories and cosmetics; 44% lack an included-accessories image; 76% of mobile sites don't use thumbnails for additional images.
- **83% of apparel sites don't provide sufficient sizing information.**

Fix the set before you art-direct the hero.

## Variant selection

**Expose options as buttons or swatches, not a drop-down.** This is Baymard's cleanest finding, with three documented failure modes: users overlook the selector entirely even beside the buy button; users discover their size is unavailable only after investing time; on mobile, repeated scrolling between colour and size causes abandonment. 71% of desktop sites now use buttons, up from 63% in 2017; 28% still use drop-downs. `(measured)`

Show unavailable variants **disabled, not removed**, so users can tell "not offered" from "sold out".

Note this is systematic moderated usability testing, not a randomized conversion experiment — the conversion delta is not quantified. It is still the strongest component-level finding available.

## Badges and social proof

**One status badge above the title**, if it is true. The mechanism people call "halo effect" is more precisely **observational learning / popularity cueing**, which has strong field evidence: labelling the most popular items raised their demand **13–20%** in a randomized field experiment. `(measured)`

`(legal)` A "Best seller" or "Most popular" label is a factual claim requiring substantiation. Wire it to a live query, define the window in the label ("sold in the last 7 days"), and be able to reproduce the number on demand. **Never invent it.** Fake indicators of social influence are separately prohibited (`bright-lines.md` L20, L22).

**Ratings:** show the real rating and real count, unrounded. Do not chase 5.0 — purchase likelihood peaks around 4.0–4.7 and declines toward perfection. Volume dominates the last decimal: five reviews carry 270% greater purchase likelihood than none. `(measured, observational)`

`(legal)` Never display a rating summary filtered by sentiment while implying it is the full set, and never suppress negative reviews while publishing positive ones (L17).

**Low-volume products should show nothing** rather than a technically true but unimpressive number.

## Page structure

- **No horizontal tabs** for main PDP sections; **no PDP subpages on mobile.** Expanded sections on desktop, vertical accordions on long mobile pages. 29% still use horizontal tabs; 26% of mobile sites push content into subpages. `(measured)`
- **Structure descriptions as scannable highlights** — 78% fail to, and the structure is measured to increase engagement.
- Content hidden behind an interaction is content users do not find. If it answers a common objection, it is not "advanced detail".

## Purchase options and subscriptions

Side-by-side selection cards beat stacked radio buttons for guiding choice — but the mechanism is **the default plus visual salience**, not the control type. "Radio buttons present equal choices so users pick the low-risk one" is an invented mechanism. `(mechanism)`

`(legal)` Before pre-selecting a subscription over a one-time purchase:

- The recurring nature, amount, frequency and cancellation route must be disclosed before the billing field (L15).
- Cancelling must be no harder than subscribing (L14).
- If a pre-selection adds any payment **beyond the main contractual obligation**, it is unlawful in the EU and the consumer is entitled to reimbursement (L3). A subscribe-vs-one-time card configures the main obligation, so Art. 22 is a strained fit — but UCPD Arts. 6–7 on misleading omission of renewal terms applies squarely.
- A pre-selection your user would not have chosen knowingly is a broken promise: users who notice discount every other default you set.

Put the reassurance **inside** the selected card — the saving, the cancellation freedom, the dispatch terms — at the moment of decision.

**Progressive disclosure of bundle tiers**: apply Nielsen's own conditions — expose anything users frequently need, and never exceed two levels. Revealing a better per-unit price only *after* the user commits to the costlier path reads as a bait pattern. The circulating "30–50% faster completion" figure is fabricated.

## Trust elements

Replace decorative badges with **verifiable answers to this buyer's actual objection**: the return window and who pays return shipping, the resolved delivery date, a link to the real lab report or certification.

- Specific-beats-generic badges is a reasoned extrapolation, not a measured finding. `(untested)`
- Generic assurances are not worthless — shipping cost is the single largest abandonment driver.
- `(legal)` A substantive product claim ("third-party tested for heavy metals") is not a badge; it requires competent and reliable substantiation under FTC Act §5.
- Perceived payment security comes from the **visual treatment of the payment section**, not from seals — 89% of sites fail to visually reinforce sensitive card fields.

## CTA

Keep it literal: **"Add to cart"**. It is the most-scanned element on the page and users pattern-match on the familiar word; ambiguous or decorated labels on primary purchase actions cause hesitation and misdirected clicks. The "Add to cart · Start my journey" advice traces to a single unpublished 2013 test. Put brand voice in the microcopy beneath the button.

## Checklist

- [ ] Shipping cost, delivery expectation and return policy are all answerable without leaving the page
- [ ] Image set covers scale, detail, context and accessories — with zoom
- [ ] Variants are exposed buttons; unavailable ones disabled, not hidden; all page data re-syncs on change
- [ ] Every badge, count and rating comes from live data and is substantiable
- [ ] No horizontal tabs; no mobile subpages; description is scannable
- [ ] Any pre-selected recurring option discloses terms before billing and is as easy to leave as to enter
- [ ] Primary CTA is conventional
