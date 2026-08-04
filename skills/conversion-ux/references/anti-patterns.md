# Anti-patterns

The catalog of what goes wrong. **If your design matches an entry, it is wrong** — fix it before shipping, regardless of what the brief asked for.

Each entry: what it looks like → why it fails → what to do instead.

---

## Fabricated evidence

**❌ Invented social proof.** "500+ sold this week", "1,247 people are viewing this", "Join 10,000+ happy customers", a testimonial trio with generated names and stock faces.
*Why it fails:* You cannot substantiate it, and a "Best seller" or popularity claim is a factual claim under FTC Act §5 and UCPD Art. 6. Fake indicators of social influence are separately prohibited (16 CFR § 465.8). Beyond the law, a number the team cannot reproduce on demand is a landmine for whoever inherits the page.
**✅ Wire the count to a live query, define the window in the label, and show nothing when the number is unimpressive.** If you are producing a mockup, mark the slot as data the user must supply — never fill it with a plausible figure.

**❌ Fake urgency.** A countdown that resets on reload, "limited time" with no end date, "only 3 left" you cannot verify.
*Why it fails:* Per-se banned commercial practice in the EU (UCPD Annex I point 7 — no consumer-harm test required) and the UK (DMCC Sch. 20 para 7), and an enumerated FTC deceptive pattern.
**✅ Real deadlines only, from real data.** If the constraint is real, say what happens at the deadline and let the user act after it.

**❌ Quoting a fabricated statistic to justify a design.** "70–90% never change defaults", "free samples lift purchases 2,000%", "trust badges convert 23% better".
*Why it fails:* All three are traceable fabrications. Repeating them makes every other claim in your rationale suspect.
**✅ Check `evidence.md` before citing any UX statistic.** State the mechanism and mark the application as untested where it is.

---

## Coercion and asymmetry

**❌ Confirmshaming.** "No thanks, I like paying full price." "I'll risk it." "No, I don't care about saving money."
*Why it fails:* Named in the FTC's dark-patterns taxonomy and Colorado Rule 7.09(A)(2). Intent is not a defence — the test is effect.
**✅ Neutral decline labels.** "Not now." And remember the decline for the rest of the visit.

**❌ Asymmetric choice.** A bright large "Accept all" beside a greyed small "Manage preferences"; accept in one click and decline in three.
*Why it fails:* Colorado Rule 7.09(A)(1) requires equal size and salience explicitly; EDPB treats the longer path as a breach.
**✅ Equal weight, equal step count, both paths visible at the same level.**

**❌ Pre-ticked consent or pre-added charges.** A checked newsletter box, a pre-added insurance add-on, a pre-enabled data-sharing toggle.
*Why it fails:* GDPR Recital 32 and CJEU *Planet49* invalidate the consent; CRD Art. 22 entitles the consumer to a **refund** of a pre-ticked extra payment.
**✅ Default effort, never permission or money.**

**❌ The exit is harder than the entrance.** Subscribe in two taps, cancel by emailing support.
*Why it fails:* DSA Art. 25(3)(c), California AB 2863, ROSCA.
**✅ Cancel in the same medium, in no more steps than signup took.**

**❌ Nagging.** Re-showing a dismissed modal on the next page; stacked popups; an interstitial that requires an action to escape.
*Why it fails:* Colorado Rule 7.09(A)(6) and DSA Art. 25(3)(b). Modal ads are the most-hated technique measured on desktop and mobile, and the discriminating variable is **forced dismissal**.
**✅ One ask, remembered. Prefer inline placement over interruption.**

---

## Hiding what the user came for

**❌ A wall in front of the first value.** Blurred results the user generated, "create an account to see your report", a login wall on a content page.
*Why it fails:* 18% of cart abandonment is forced account creation. NN/g's login-wall research is unusually blunt about how much users hate it.
**✅ Move the wall, don't remove it — deliver a real partial result, then ask.** And preserve everything the user built through signup.

**❌ Price on request.** No pricing anywhere, or a multi-input calculator instead of numbers.
*Why it fails:* Pricing is the #1 information need in B2B research; participants abandoned and went to competitors, and read concealment as evasiveness. Calculators tested as complex and error-prone.
**✅ Sample prices for representative scenarios, then ranges, then MSRP.**

**❌ Shipping cost, delivery date or return policy discoverable only at checkout.**
*Why it fails:* 64% look for shipping cost on the product page and 43% of sites don't provide it; 60% look for the return policy there and 44% don't. Extra costs are the single largest abandonment reason at 40%.
**✅ Answer all three on the product page.**

**❌ Task-critical information inside a tooltip.** Password rules, fee explanations, plan limits, format requirements on hover.
*Why it fails:* Tooltips must never carry information required to complete the task; they fail on touch, on keyboard, and on small screens.
**✅ Put it inline, visible, before the user needs it.**

**❌ Three levels of disclosure.** Accordion inside a tab inside a "show more".
*Why it fails:* Nielsen's own condition is never exceed two levels. Content hidden behind an interaction is content users do not find.
**✅ Two levels maximum, and only where the expected user reads a minority of sections.**

---

## Interface defaults that cost money

**❌ Drop-down for variant selection.** Size or colour hidden behind a select.
*Why it fails:* Users overlook the selector entirely, discover unavailability only after investing time, and abandon on mobile from repeated scrolling between selectors. 71% of desktop sites have already moved to buttons.
**✅ Exposed buttons or swatches. Unavailable variants disabled, not removed.**

**❌ A slider as the only way to set a number.** Price filters, quantity, dates.
*Why it fails:* 83% of slider implementations apply a linear scale to non-linearly distributed data; over half of test subjects misread dual-handle sliders. WCAG 2.2 SC 2.5.7 names range sliders explicitly and requires a non-drag alternative.
**✅ Link a slider to a text field showing the same value, make the track clickable, support arrow keys.**

**❌ Placeholder text as the label.** The label vanishes on focus.
*Why it fails:* Strains memory, prevents pre-submit verification, is mistaken for a pre-filled value, announces unreliably to screen readers.
**✅ Persistent visible label above every field.**

**❌ Wiping the form on a validation error.** Especially the card number.
*Why it fails:* Documented abandonment cause, and a WCAG 2.2 **Level A** failure — the lowest bar there is. 34% of sites still do it with card fields.
**✅ Preserve every value; highlight only what needs fixing.**

**❌ Generic validation messages.** "Please enter a valid phone number."
*Why it fails:* 98% of sites do this and only 2% target the actual violation. Participants took up to five minutes to resolve trivial errors.
**✅ Name the actual violation. Author 4–7 messages for the most complex fields.**

**❌ CAPTCHA or password complexity rules in checkout.**
*Why it fails:* 19% abandonment from password rules; CAPTCHA failure rates of 8%, rising to 29% when case-sensitive; puzzle CAPTCHAs also fail WCAG 2.2 SC 3.3.8.
**✅ Remove both. Move friction to account creation on the confirmation page.**

**❌ Horizontal tabs for main product-page sections, or pushing content to mobile subpages.**
*Why it fails:* Vertical scroll moves attention away from non-sticky tabs; 29% and 26% of sites respectively still do it.
**✅ Expanded sections on desktop, vertical accordions on long mobile pages.**

---

## Composition

**❌ The generic AI conversion page.** Three-column pricing table with "Most popular" on the middle tier, green-check bullets, gradient hero, invented testimonial trio, grey trust-badge row, countdown starting on page load.
*Why it fails:* It is what every run produces for every brief, so it communicates nothing about this product, and half its components are the fabricated-evidence and fake-urgency entries above.
**✅ Earn each element from the brief.** In the space the badge row would have occupied, put the total price, the delivery date, or the cancellation terms — the things users are actually hunting for.

**❌ A decorative hero above a paywall or product page.** Beautiful illustration; no information.
*Why it fails:* It does not answer "what am I getting?", and users cannot commit to something they cannot visualise. On a product page, 42% of users judge physical size from images alone.
**✅ Show the real product, and ship an image set rather than one better hero.**

**❌ Copying a competitor's checkout.** "This is how Amazon does it."
*Why it fails:* 64% of desktop, 63% of mobile and 46% of app checkouts rate mediocre or worse in the benchmark; **zero** reach state-of-the-art. The modal competitor is failing.
**✅ Check against explicit guidelines, not observed practice.**
