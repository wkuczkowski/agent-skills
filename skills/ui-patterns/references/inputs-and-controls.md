# Inputs and controls

## The real question: exploring or stating?

The useful axis is not "how often is this entered" but **whether the user is exploring a value or stating one they already know**. `(measured)`

- **Exploring** — "show me something around £40", "roughly how long a trip?" — a slider or a picker works, because approximate is the point and feedback is immediate.
- **Stating** — "350 g", "my weight is 78.4", "quantity 12" — a text field or stepper, because the user already has the number and any control that makes them hunt for it is friction.

Frequency correlates with this but is not the rule. A one-time input can still be a known value: someone entering their height at signup knows it exactly, and making them spin a wheel to reach it is worse, not gentler.

**NN/g's actual recommendation is a linked control, not an either/or**: a slider bound to a text field showing the same value, each updating the other. That is stronger than choosing one.

## Sliders

**Never ship a slider as the only way to set a number.**

Why they fail in practice: `(measured)`

- **83% of top-50 sites using sliders apply a linear scale to non-linearly distributed data** — so roughly half the track controls 2–10% of the outcomes and the useful range is compressed into a few pixels.
- **Over 50% of test subjects misread dual-handle range sliders as single-point controls.** That is a comprehension failure, not a motor one — it does not go away with bigger handles.
- Handles test as overly sensitive; subjects needed several attempts to hit a target.

Baymard's position, which is worth adopting verbatim: if you cannot implement non-linear scaling, visually distinct dual handles, click-to-position on the track, and a text fallback, **do not use a slider at all**.

Accessibility is not optional here: `(legal)`

- **WCAG 2.2 SC 2.5.7 Dragging Movements (AA) names range sliders explicitly** and requires a single-pointer, non-dragging alternative. A clickable track plus arrow-key stepping satisfies it.
- Expose `role="slider"` with `aria-valuenow`, `aria-valuemin`, `aria-valuemax` and `aria-valuetext`, so a screen reader announces "£40, medium" rather than "40".
- Handle and track must meet target-size and non-text-contrast requirements.

"Sliders are low effort because users don't have to type" is true only for a pointer user with typical motor control, and inverts for several groups.

## Wheel pickers

Fine for roughly 60 values; unusable at 1,000. Apple's own guidance warns that long picker lists are tedious to navigate and recommends a list or table with an index for large sets. `(qualitative)`

Wheel pickers also hide the range: the user cannot see the minimum, maximum, or where they are within it without scrolling. If the range matters, that is the wrong control.

## Number fields

Use a text input with the right keyboard and the right constraints. Set `inputmode` so mobile users get a numeric keypad, and use `autocomplete` tokens where the field collects information about the user — WCAG 2.1 SC 1.3.5 makes this a conformance issue for user-information fields, and `autocomplete` is the sufficient technique.

**Accept the format the user types and clean it up** rather than rejecting it: spaces in card numbers, dashes in phone numbers, units after a figure. 80% of sites don't allow or auto-format spaces in card numbers.

**A caution:** the design intent here — typed entry for precise values — is sound, but the native `<input type="number">` element has known behavioural defects across browsers (scroll-wheel value changes, silent rejection of non-numeric input, inconsistent spinner behaviour, locale handling). This was flagged as unverified in the research behind this skill. Before defaulting to it, check the current GOV.UK Design System guidance and MDN, and consider `inputmode="numeric"` on a text input instead.

## Steppers

Right for small integer quantities the user adjusts relative to a current value. Pair with an open text field for larger jumps — 61% of cart implementations don't use buttons at all, and 22% wrongly require an "Apply" button after the change. **Quantity changes should apply immediately**, with confirmation appearing near the control the user touched.

## Drop-downs vs radios vs autocomplete

`(measured)` **Under ~5 options → radio buttons or a segmented control. 5–10 → drop-down acceptable. Over ~10 → autocomplete, lookup, or auto-detect.**

The reason: **55% of users open a drop-down purely to see what is in it and immediately close it** — a wasted interaction that a visible control eliminates. Above roughly 10 uncategorized options there is no overview at all, and scrolling hides options or causes mis-selection.

**Do not conclude that drop-downs are lazy design.** They remain correct when the user does not know the option set and cannot type it — country and state selectors are drop-downs on purpose. Prefer **native** selects when a select is warranted: 31% of sites with custom-built drop-downs have usability defects in them.

For variant selection on a product page, exposed buttons or swatches win outright — see the `conversion-ux` skill.

## Radio buttons

**Pre-select one option by default.** A radio cannot be deselected once clicked, so an all-unselected group traps a user who changes their mind and wants to return to "none". A default also guides users through unfamiliar options.

Exceptions: you genuinely do not know the preference, the pre-selection risks offending (gender, title, pronouns), or law forbids it — consent, legal declarations and anything that adds a charge must never be defaulted.

There is **no published threshold** for what counts as a dominant enough choice to default. If someone asks for a percentage, the answer is "instrument it", not an invented number.

## Checkboxes

- Default **unchecked** for anything promotional, legal, or consent-related. Pre-checking is a deceptive pattern and, for consent, invalidates it.
- Square with a checkmark, never circles — circles read as radio buttons and imply mutual exclusivity.
- The **label is clickable**, with a touch target of at least 1 cm × 1 cm.
- Positively worded labels, so the user never has to resolve a double negative.
- Vertical lists; state min/max selection requirements explicitly; use the indeterminate state for parent checkboxes.

Auditable rule: any `input[type=checkbox][checked]` whose label mentions marketing, newsletter, terms, sharing or consent is a defect.

## Text fields, generally

- **Persistent visible label above the field.** Placeholders as labels strain memory, prevent pre-submit verification, are mistaken for pre-filled values, and announce unreliably.
- **Field width signals expected input length** — a postcode field the width of a street address invites the wrong thing.
- **Single column.** Multiple columns interrupt vertical momentum; City/State/Zip on one row is the standard exception.
- **Validate on blur**, or once the character count is correct for fixed-length inputs. Never validate mid-typing on a value that will become valid. Clear the error the instant the input becomes valid.
- **Preserve input on failed submit.** This is a WCAG 2.2 Level A requirement, not a nicety.
- **No Reset or Clear buttons.** The risk of accidental deletion outweighs the unlikely need to start over.
