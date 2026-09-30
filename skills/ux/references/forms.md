# Forms, validation and errors

Rules for any form, from a signup to an internal data-entry screen. Checkout-specific counts (field budget, guest checkout, address, card fields) are in `checkout.md`; individual controls (sliders, pickers, drop-downs, radios, checkboxes, number fields) in `inputs-and-controls.md`.

Forms built to usability guidelines achieved **78% one-try error-free submissions vs 42%** for forms violating them — a controlled study, and the strongest single number in the forms literature (Seckler et al., CHI '14). `(measured)`

## Reduce before you polish — EAS

1. **Eliminate** — for each field, why is this needed? Defer the non-urgent; use conditional logic so only relevant questions appear.
2. **Automate** — prefill from prior submissions and integrations; infer city/state from postcode, age from date of birth.
3. **Simplify** — sensible defaults; alternative inputs (camera scan, GPS); accept any format and clean it up rather than rejecting (spaces in card numbers, dashes in phone numbers, units after a figure).

Prettifying a field that should not exist is wasted work.

## Required vs optional

- **Mark both** — asterisk for required *and* a literal "(Optional)". 98% of sites have at least one optional field; only **14% desktop / 6% mobile** mark both. 32% of users hit a validation error from an unmarked required field. `(measured)`
- **Every required field is either made optional or given a one-line justification.** 14% abandon if "phone" is required with no explanation; over 70% report reluctance to give a phone number. The finding that matters: **no abandonment was observed when the same fields were optional** — users simply left them blank. With an inline explanation ("Only used to contact you about problems with your delivery") subjects willingly provided the data. `(measured)`

## Layout and labels

- **Single column.** Multiple columns interrupt vertical momentum; City/State/Zip on one row is the standard exception.
- **Persistent visible label above the field**; never placeholder-as-label (`bright-lines.md` A8).
- **Field width signals expected input length** — a postcode field the width of a street address invites the wrong thing.
- Sequence by familiarity, priority, dependency, complexity — and **ask sensitive items late**.
- Target 6th–8th grade reading level on labels and helper text.
- **No Reset or Clear buttons.** The risk of accidental deletion outweighs the unlikely need to start over.

## Validation and errors

- **Validate on blur**, or for fixed-length inputs (postcode, phone, card) once the character count is correct. Never validate while the user is still typing something that will become valid. The error disappears the instant the input becomes valid. 32% of sites provide no field validation at all. `(measured)`
- **Name the actual violation.** "Phone number can only contain numbers" — not "Please enter a valid phone number". 98% of sites use generic messages; only 2% target the actual violation, and participants took **up to five minutes** to resolve trivial errors. Author 4–7 distinct messages for the most complex inputs. `(measured)`
- **Inline, per field, adjacent to the offending input.** Never a top-of-form summary alone. Never a hover or focus tooltip.
- **Text + icon + colour**, never colour alone (`bright-lines.md` A7). Distinct visual states for error, warning and success.
- Confirm success inline for complex fields, not only failure.
- **Preserve the user's input** on a failed submit — a documented abandonment cause and a WCAG 2.2 Level A failure (`bright-lines.md` A3).
- When users hit the same error **three or more times, treat it as a design defect**, not user error.

**Slips vs mistakes need different fixes.** *Slips* are autopilot errors: fix with constraints (a date picker that cannot select a return before departure), good defaults, and forgiving formatting that accepts any input shape and reformats it. *Mistakes* are wrong mental models: fix with conventions, clear signifiers, a preview before commitment, and keeping context visible across multi-step flows.

**Confirmation policy:** undo by default; confirmation reserved for the irreversible; escalated friction (re-entering a password) only for the catastrophic. Blanket confirmation dialogs produce habituation and protect nobody.

## Multi-step flows

Use a wizard for novice or infrequent users doing setup, not for repeated expert tasks. Show a **visual list of steps with the current one highlighted** so users can gauge length and position. Enforce sequential ordering. Replace "Next" with a label stating what happens next ("Review order"). **Allow mid-process exit with state saved.** Make each step self-sufficient so the user never leaves to fetch data. For long forms, give an estimated completion time and a pre-flight list of what they will need. Progress indicators pace fast-to-slow (`principles.md` §3). Test the Back button at every step (`adaptive-and-post-purchase.md`).
