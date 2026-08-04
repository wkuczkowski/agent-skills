# Sweeps

The finding pass. Run only the sweeps for surfaces actually present in scope. Every hit is a *candidate* — it still has to survive stage 2 and the evidence contract.

## Universal sweep — run on any screen

**The question test.** Walk each element and ask: *what question is this making the user answer?* Flag any element posing a hard question (*is this worth it? what will this cost in total? what am I committing to?*) where the screen could answer it instead, and any element posing no question at all — decoration occupying the position of information.

**The unanswered objection.** Name the one thing that would make this user say no. Search the screen for the answer. If it is not there, that is usually the highest-value finding on the page.

**The blank-slot test.** Any number on screen — counts, ratings, timers, stock levels, "N people viewing" — trace it to a source. A number nobody can reproduce is a finding, and if it is fabricated it is a P0 (see `deceptive-patterns.md`).

**States you have not seen.** Note them rather than guessing: hover, focus, active, disabled, loading, empty, error, long content, zero results, dark mode, 320px width, 200% zoom.

## Forms and checkout

- Count the fields. Industry average is 11.3; the achievable target is 8. Every field above that needs a justification.
- Any field the user will find intrusive (phone, date of birth, company) without a one-line reason next to it.
- Required vs optional: are **both** marked? Only 14% of desktop and 6% of mobile sites mark both.
- Guest checkout: present, labelled with the word "Guest", most prominent, above sign-in, inside the first mobile viewport?
- Labels: persistent and visible, or placeholders doing the job?
- Single column? Multi-column forms interrupt vertical momentum.
- Validation timing — does it fire while the user is still typing something that will become valid?
- Error messages: do they name the actual violation, sit next to the field, and use text + icon + colour?
- **Does a failed submit preserve every entered value?** Losing the card number is a WCAG 2.2 Level A failure.
- Is submission idempotent — what happens on a double-click?
- CAPTCHA or password-complexity rules present in checkout?
- Reset or Clear button present?
- Address entry: lookup available, and do conventional fields remain as a fallback when it fails?
- Card fields: spaces accepted, expiry formatted MM/YY, sensitive fields visually reinforced?

## Product and pricing pages

- Shipping cost answerable on this page?
- Return policy answerable on this page?
- Delivery expectation given as a **resolved date**, not a shipping speed?
- Total price visible before the billing step?
- Variant selection: exposed buttons or a drop-down? Unavailable variants disabled or removed?
- Does everything re-sync on variant change — gallery, price, stock, specs, reviews?
- Image set: scale reference, detail, context, accessories, zoom — or one hero doing all the work?
- Every badge and rating traceable to live data?
- Horizontal tabs on a long page, or content pushed to a mobile subpage?
- Pricing: one number per plan or a range? More than five plans compared? Any empty cells in the comparison table? Do headers stick?
- Any struck-through reference price — and can the team prove it is the lowest price of the previous 30 days?

## Paywalls, trials, upgrade prompts

- Is the screen asking "is this worth it?" when it could ask "can I try this?"
- Charge date, amount and cancellation route stated **before** any billing field?
- Any promised reminder — is it actually built? On iOS there is no automatic trial-end reminder.
- Dismiss label neutral, or shaming?
- Is the decline remembered, or will the user see this again on the next screen?
- Cancellation: same medium, no more steps than signup?
- Hero showing the real product or decoration?

## Onboarding and signup

- Is there a wall in front of the first value?
- Is state built before signup preserved through it?
- Progress indicator: does it pace fast-to-slow? Slow-to-fast pacing measurably *increases* abandonment.
- Step count visible, and can the user leave and resume?
- Is each step self-sufficient, or must the user leave to fetch information?
- Are any defaulted fields consent, legal declarations, or charges?

## Search and browse

- Run the eight query types: exact, product type, symptom, feature, use case, compatibility, abbreviation, non-product. Failure rates in the wild run from 12% to 66% along that axis.
- Does search cover non-product content — returns, order status, contact? 34% of users try it; 15% of sites do not support it.
- Autocomplete: capped at ~10 desktop / ~8 mobile, matched text highlighted, active suggestion copied into the field on arrow-down?
- No-results page: does it offer recovery with **previewed** alternatives, or a dead end? Roughly half of sites offer nothing.
- Is the query preserved in the field after submitting?
- Category tiles: text over imagery — does contrast hold at the *worst* point of each image?

## Empty and error states

- Does the empty state communicate status, teach something, and offer a direct action?
- Is a "no records" message shown while data is still loading?
- Are the first-run and filtered-to-nothing states distinguished?
- Do error states leave a route forward, or just report failure?

## Accessibility pass

Not a substitute for axe or Lighthouse — those are excluded findings. Look for what tooling misses:

- Drag-only interactions, especially range sliders (WCAG 2.2 SC 2.5.7 names them).
- Targets under 24 × 24 CSS px with insufficient spacing.
- Task-critical content in a hover-only tooltip.
- Paste blocked on a credential or code field (SC 3.3.8).
- Information re-requested that was already entered in the same process (SC 3.3.7).
- Meaning carried by colour alone.
- Text over images where contrast varies across the image.
- Time limits without an extension route.

## Mobile pass

Run separately — the mobile experience is measurably worse across the industry, and a desktop audit does not cover it.

- Reachability of the primary action with one thumb.
- Correct keyboard for each input (`inputmode`, `type`).
- Anything requiring hover.
- Horizontal scrolling anywhere it is not deliberate.
- Sticky elements eating the viewport.
- Back-button behaviour at every step — 59% of sites violate expectations here.
