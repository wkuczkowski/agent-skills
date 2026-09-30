# Cart and checkout

The best-evidenced area in this skill; almost every rule here has a countable denominator behind it. General form rules (required/optional marking, layout, validation, error messages, multi-step flows) are in `forms.md` and apply here in full.

## Start from the abandonment reasons

Cart abandonment baseline is **70.2%**, computed across 50 published studies over 14 years. Ranked fixable reasons, excluding "just browsing": `(measured)`

| Reason | Share |
|---|---|
| Extra costs too high (shipping, tax, fees) | **40%** |
| Delivery too slow | 20% |
| Didn't trust the site with card details | 19% |
| Site required account creation | 18% |
| Checkout too long or complicated | 17% |
| Site errors or crashes | 17% |
| Unsatisfactory returns policy | 13% |
| Couldn't see or calculate total cost up front | 12% |
| Card declined | 10% |
| Insufficient payment methods | 9% |

**Realistic ceiling: up to a 35% relative conversion increase** from checkout design alone on a large site, with the average site needing **32 discrete improvements**. Price expectations against that, not against a 3.5× story.

## Count the fields

Industry average is **5.1 steps and 11.3 fields**; the achievable target is **8 fields**. Named reductions with measured effects: `(measured)`

- **One "Full Name" field.** 89% of sites use more than one; 42% of participants typed their full name into "First Name" at least once. With a single field only 4% hesitated.
- **Hide "Address Line 2"** behind a link. 75% show it by default; 30% of users came to a full stop on reaching it.
- **Hide the coupon field** behind a link. 70% have one; 35% of those show it by default.
- **Default billing = shipping.** 24% of sites assume different by default.
- **Defer account creation to the confirmation page.** 42–54% don't (`adaptive-and-post-purchase.md`, "The confirmation page").

## Guest checkout

Must be the **single most prominent option**, labelled with the word "Guest", above sign-in, inside the initial mobile viewport. 47% of sites (62% in the newer wave) fail this. Named failures: labelling it "Continue" instead of "Guest"; rendering it as a subdued text link; placing it below sign-in; email-first flows. `(measured)`

Users associate registration with spam. 57% of sites fail to state concrete account benefits, and vague benefit copy performs nearly as poorly as none.

## Delivery and cost

**Show a resolved delivery date, not a shipping speed.** 41–48% of sites don't. Observed behaviour: users counting calendar days and opening desktop calendars; identical speed labels producing different arrival conclusions; confusion over cutoffs, business days and weekends. Windows wider than about 5–7 days caused hesitation on their own. 83% fail to show the order cutoff as a countdown. `(measured)`

On total cost, read the honest position in `evidence.md` — the only large randomized field experiment found hiding fees until checkout *raised* revenue. Show the all-in total early, and justify it as compliance and trust, not as a conversion win. `(legal)` In the UK all B2C invitations to purchase must carry the total price or how it is calculated (`bright-lines.md` L26); in US live-event ticketing and short-term lodging the total must be the most prominent price (L25).

## Address

Fully automatic lookup, tolerant of misspellings, ranked by **geographic proximity, not alphabetically** — and conventional fields must stay visible as a fallback. 55% have no automatic lookup; 47% have no validator; 28% of mobile sites don't autodetect city and state from postal code. Critically, **69% of participants ended up completing the address manually when suggestions failed** — a lookup-only interface strands them. `(measured)`

## Card fields

`(measured)` — every one of these is a countable site failure:

- **80% don't allow or auto-format spaces** in the card number; 15% offer no autoformatting at all.
- **72% don't format expiry as MM/YY** like the physical card.
- **34% don't retain card-field data after a validation error** — which is now also a WCAG 2.2 **Level A** failure (`bright-lines.md` A3).
- **89% don't visually reinforce the sensitive card fields.** Perceived security comes from that treatment, not from seals.
- 21% accept only one payment method.
- Users still double-click submit — **submission must be idempotent**.
- 22% wrongly attach "Apply" buttons to fields that should apply immediately.

## Friction to delete outright

- **No complex password requirements in checkout** — 19% abandonment, and 65–82% of sites impose them.
- **No CAPTCHA in checkout.** Failure rate 8%, rising to 29% when case-sensitive. `(legal)` Puzzle CAPTCHAs also fail WCAG 2.2 SC 3.3.8 (`bright-lines.md` A2).
- **No Reset or Clear buttons.**

## Cart

Quantity via buttons, or buttons plus an open text field; changes apply **immediately**; confirmation appears near the input the user touched (`inputs-and-controls.md`, "Steppers").

## Checklist

- [ ] Field count is at or below 8, and every remaining field has a reason
- [ ] Guest checkout is the most prominent option and says "Guest"
- [ ] Required and optional both marked; unusual fields carry a one-line justification
- [ ] Resolved delivery date shown, not a shipping speed
- [ ] Card fields autoformat, survive errors, and are visually reinforced
- [ ] Validation on blur; messages name the actual violation and sit next to the field
- [ ] No CAPTCHA, no password complexity rules, no Reset button in checkout
- [ ] Submission is idempotent
