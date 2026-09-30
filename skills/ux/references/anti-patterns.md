# Anti-patterns

The final check for any build. A design that matches an entry is wrong regardless of what the brief asked for. Each entry: what it looks like → what to do instead, with the file that carries the evidence. The deceptive and unlawful patterns (invented social proof, fake urgency, confirmshaming, asymmetric choice, pre-ticked consent or charges, a hard exit, nagging) are in `bright-lines.md`, under "What the compliant version looks like"; check them there as part of this pass.

## Fabricated evidence

**Quoting a fabricated statistic to justify a design** — "70–90% never change defaults", "free samples lift purchases 2,000%", "trust badges convert 23% better". All three are traceable fabrications, and repeating one makes every other claim in the rationale suspect. → Check `evidence.md` before citing any UX statistic; state the mechanism and mark the application as untested where it is.

## Hiding what the user came for

**A wall in front of the first value** — blurred results the user generated, "create an account to see your report", a login wall on a content page. 18% of cart abandonment is forced account creation, and NN/g's login-wall research is unusually blunt about how much users hate it. → Move the wall, don't remove it: deliver a real partial result, then ask, and preserve everything the user built through signup (`principles.md` §3–4).

**Price on request** — no pricing anywhere, or a multi-input calculator instead of numbers. → Sample prices for representative scenarios, then ranges, then MSRP (`pricing-and-plans.md`).

**Shipping cost, delivery date or return policy discoverable only at checkout.** → Answer all three on the product page (`product-pages.md`, `checkout.md`).

**Task-critical information inside a tooltip** — password rules, fee explanations, plan limits, format requirements on hover. Tooltips fail on touch, on keyboard and on small screens. → Inline, visible, before the user needs it (`disclosure-and-feedback.md`).

**Three levels of disclosure** — accordion inside a tab inside a "show more". → Two levels maximum, and only where the expected user reads a minority of sections (`disclosure-and-feedback.md`).

## Interface defaults that cost money

**Drop-down for variant selection.** → Exposed buttons or swatches; unavailable variants disabled, not removed (`product-pages.md`).

**A slider as the only way to set a number** — price filters, quantity, dates. → A slider linked to a text field showing the same value, a clickable track, arrow keys (`inputs-and-controls.md`, `bright-lines.md` A5).

**Placeholder text as the label.** → Persistent visible label above every field (`bright-lines.md` A8).

**Wiping the form on a validation error**, especially the card number. → Preserve every value; highlight only what needs fixing (`forms.md`, `bright-lines.md` A3).

**Generic validation messages** — "Please enter a valid phone number". → Name the actual violation; author 4–7 messages for the most complex fields (`forms.md`).

**CAPTCHA or password complexity rules in checkout.** → Remove both; move friction to account creation on the confirmation page (`checkout.md`).

**Horizontal tabs for main product-page sections, or content pushed to mobile subpages.** → Expanded sections on desktop, vertical accordions on long mobile pages (`product-pages.md`).

## Composition

**The generic AI conversion page** — three-column pricing table with "Most popular" on the middle tier, green-check feature bullets, gradient hero, invented testimonial trio, grey trust-badge row, "Join 10,000+ happy customers" with no source, countdown starting on page load. It is what every run produces for every brief, so it communicates nothing about this product, and half its components are the fabricated-evidence and fake-urgency patterns. → Earn each element from the brief and back it with real data. In the space the badge row would have occupied, put the total price, the delivery date, the cancellation terms, or what happens next — the things users are actually hunting for.

**A decorative hero above a paywall or product page** — beautiful illustration, no information. It does not answer "what am I getting?", and users cannot commit to something they cannot visualise. → Show the real product, and ship an image set rather than one better hero (`product-pages.md`).

**Copying a competitor's checkout** — "this is how Amazon does it". 64% of desktop, 63% of mobile and 46% of app checkouts rate mediocre or worse in Baymard's benchmark; **zero** reach state-of-the-art, so the modal competitor is failing. → Check against explicit guidelines, not observed practice.
