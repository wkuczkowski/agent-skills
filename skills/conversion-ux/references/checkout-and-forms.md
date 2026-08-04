# Cart, checkout and forms

The best-evidenced part of this whole skill. Almost every rule here has a countable denominator behind it.

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
- **Defer account creation to the confirmation page.** 42–54% don't.

## Guest checkout

Must be the **single most prominent option**, labelled with the word "Guest", above sign-in, inside the initial mobile viewport. 47% of sites (62% in the newer wave) fail this. Named failures: labelling it "Continue" instead of "Guest"; rendering it as a subdued text link; placing it below sign-in; email-first flows. `(measured)`

Users associate registration with spam. 57% of sites fail to state concrete account benefits, and vague benefit copy performs nearly as poorly as none.

## Required vs optional

- **Mark both** — asterisk for required *and* a literal "(Optional)". 98% of sites have at least one optional field; only **14% desktop / 6% mobile** mark both. 32% of users hit a validation error from an unmarked required field. `(measured)`
- **Every required field is either made optional or given a one-line justification.** 14% abandon if "phone" is required with no explanation; over 70% report reluctance to give a phone number. The finding that matters: **no abandonment was observed when the same fields were optional** — users simply left them blank. With an inline explanation ("Only used to contact you about problems with your delivery") subjects willingly provided the data. `(measured)`

## Delivery and cost

**Show a resolved delivery date, not a shipping speed.** 41–48% of sites don't. Observed behaviour: users counting calendar days and opening desktop calendars; identical speed labels producing different arrival conclusions; confusion over cutoffs, business days and weekends. Windows wider than about 5–7 days caused hesitation on their own. 83% fail to show the order cutoff as a countdown. `(measured)`

On total cost, read the honest position in `evidence.md` — the only large randomized field experiment found hiding fees until checkout *raised* revenue. Show the all-in total early, and justify it as compliance and trust, not as a conversion win. `(legal)` In the UK all B2C invitations to purchase must carry the total price or how it is calculated (L26); in US live-event ticketing and short-term lodging the total must be the most prominent price (L25).

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
- **No CAPTCHA in checkout.** Failure rate 8%, rising to 29% when case-sensitive. `(legal)` Puzzle CAPTCHAs also fail WCAG 2.2 SC 3.3.8 (A2).
- **No Reset or Clear buttons.** The risk of accidental deletion outweighs the unlikely need to start over.

## Form construction

Forms built to usability guidelines achieved **78% one-try error-free submissions vs 42%** for forms violating them — a controlled study, and the strongest single number in the forms literature (Seckler et al., CHI '14). `(measured)`

**Reduce in this order — EAS:**

1. **Eliminate** — for each field, why is this needed? Defer the non-urgent; use conditional logic so only relevant questions appear.
2. **Automate** — prefill from prior submissions and integrations; infer city/state from postcode, age from date of birth.
3. **Simplify** — sensible defaults; alternative inputs (camera scan, GPS); accept any format and clean it up rather than rejecting.

Prettifying a field that should not exist is wasted work.

**Layout and labels:** single column (multiple columns interrupt vertical momentum; City/State/Zip on one row is the exception). Persistent visible label above the field. Never placeholder-as-label. Field width signals expected input length. Sequence by familiarity, priority, dependency, complexity — and **ask sensitive items late**. Target 6th–8th grade reading level on labels and helper text.

## Validation and errors

- **Validate on blur**, or for fixed-length inputs (postcode, phone, card) once the character count is correct. Never validate while the user is still typing something that will become valid. Error messages must disappear the instant the input becomes valid. 32% of sites provide no field validation at all. `(measured)`
- **Adaptive messages.** 98% of sites use generic ones; only 2% target the actual violation. "Phone number can only contain numbers" — not "Provide a valid phone number". Participants took **up to five minutes** to resolve simple errors. Author 4–7 distinct messages for the most complex inputs. `(measured)`
- Place the message adjacent to the offending field. Text **plus icon plus colour**, never colour alone. Never deliver errors via hover or focus tooltips. Never rely on a top-of-form summary alone.
- **Preserve the user's input** on a failed submit — a documented abandonment cause and a WCAG 2.2 Level A failure.
- When users hit the same error **three or more times, treat it as a design defect**, not a user error.

**Slips vs mistakes need different fixes.** Slips are autopilot errors — fix with constraints (a date picker that cannot select a return before departure), good defaults and forgiving formatting. Mistakes are wrong mental models — fix with conventions, clear signifiers, previews before commitment, and keeping context visible across steps.

**Confirmation policy:** undo by default; confirmation reserved for the irreversible; escalated friction only for the catastrophic. Blanket confirmation dialogs produce habituation and protect nobody.

## Multi-step flows

Use a wizard for novice or infrequent users doing setup, not for repeated expert tasks. Show a **visual list of steps with the current one highlighted** so users can gauge length and position. Enforce sequential ordering. Replace "Next" with a label stating what happens next ("Review order"). **Allow mid-process exit with state saved.** Make each step self-sufficient so the user never leaves to fetch data. For long forms, give an estimated completion time and a pre-flight list of what they will need.

## Cart

Quantity via buttons, or buttons plus an open text field; changes apply **immediately**; confirmation appears near the input the user touched. 61% don't use buttons; 22% wrongly require an "Apply" button.

## Checklist

- [ ] Field count is at or below 8, and every remaining field has a reason
- [ ] Guest checkout is the most prominent option and says "Guest"
- [ ] Required and optional both marked; unusual fields carry a one-line justification
- [ ] Resolved delivery date shown, not a shipping speed
- [ ] Card fields autoformat, survive errors, and are visually reinforced
- [ ] Validation on blur; messages name the actual violation and sit next to the field
- [ ] No CAPTCHA, no password complexity rules, no Reset button in checkout
- [ ] Submission is idempotent
