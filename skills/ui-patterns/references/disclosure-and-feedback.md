# Disclosure, empty states and feedback

## Empty states do three jobs

A blank area with a friendly illustration and a witty line fails all three. `(qualitative)`

1. **Communicate system status.** "There are no records for the selected date range" — not just whitespace. And **never show a "no records" message that later populates**; if data is loading, say so.
2. **Provide a learning cue at the moment it is relevant.** "Star an item to keep it here" is more memorable than the same fact in an upfront tutorial, because the user is standing in the situation it describes.
3. **Offer a direct path to the action that fills the state.** An actual button, not a description of what the user could do.

A first-run empty state and a filtered-to-nothing empty state are different screens with different jobs. The first teaches; the second offers a way back.

## Progressive disclosure

The circulating figures for progressive disclosure — "30–50% faster task completion", "70–90% feature discoverability" — are **fabricated**, traceable to AI-generated blog posts. Nielsen's canonical article contains no measured effect at all.

What survives are his own conditions, which cut against most uses of the pattern: `(qualitative)`

- **Expose up front everything users frequently need.** If it answers a common question, it is not advanced detail.
- **Never exceed two levels.**
- Label the trigger so hidden content is predictable — the user should know what is behind it before clicking.

**Content hidden behind an interaction is content users do not find.** Treat every disclosure as a bet that the majority does not need this.

## Accordions

Not a free win. They suit pages where users need only a few topics, and they help on small screens. They **hurt** when the audience needs most of the content — clicking headings one at a time is cumbersome, adds a decision per section, and users generally prefer scrolling well-organized content. `(qualitative)`

**Decision rule: use an accordion only when the expected user reads a minority of sections.** For a very long page, evaluate restructuring into several pages before reaching for an accordion.

Never put horizontal tabs on a long scrolling page — vertical scroll carries attention away from the tabs, and non-sticky tabs leave the viewport entirely.

## Tooltips

**Tooltips must never carry information required to complete the task.** This is a hard prohibition, and password rules, fee explanations and plan limits are where it is broken most often.

Requirements: `(qualitative)`

- Content must be genuinely additive — not a restatement of the label it hangs off.
- Must trigger on **keyboard focus** as well as hover.
- Bind to the element with an arrow; apply consistently across the interface.
- Timing: visual feedback within **0.1 s**; a **0.3–0.5 s** hover pause before exposing content; hide **0.5 s or more** after the cursor leaves. Click-revealed content appears within 0.1 s and stays until an outside click.

On touch devices hover does not exist. If the content matters, it is inline content.

## Modals and interruption

**Modal ads are the most-hated technique measured**, on both desktop and mobile — 452 US adults, average dislike 5.23 on a 7-point scale, mobile significantly worse (p < 0.0001). `(measured)`

The discriminating variable is **forced dismissal**: formats requiring a dismissal action scored worst; formats leaving control with the user scored best. This generalises directly to newsletter popups, exit-intent modals, upsell interstitials and cookie walls.

Rules:

- One ask per visit, remembered. Re-prompting a user who already declined is a rule violation in Colorado and under the DSA.
- Prefer inline placement over interruption. An inline banner that scrolls past costs nothing; a modal costs the user an action.
- Reserve modals for what genuinely blocks: a destructive confirmation, a required legal acknowledgement.
- Do not trigger browser-native `alert()`/`confirm()` for routine flows — habituation makes them worthless, and they block everything.

## Errors and confirmation

- **Inline, per field, adjacent to the offending input.** Never a top-of-form summary alone. Never a hover or focus tooltip.
- **Text + icon + colour**, never colour alone. Distinct visual states for error, warning and success.
- **Name the actual violation.** "Phone number can only contain numbers" — not "Please enter a valid phone number". 98% of sites use generic messages; only 2% target the violation, and participants have taken up to five minutes to resolve trivial errors. `(measured)`
- Confirm success inline for complex fields, not only failure.
- **When users hit the same error three or more times, treat it as a design defect**, not user error.

**Slips vs mistakes need different fixes.** *Slips* are autopilot errors: fix with constraints (a date picker that cannot select a return before departure), good defaults, and forgiving formatting that accepts any input shape and reformats it. *Mistakes* are wrong mental models: fix with conventions, clear signifiers, a preview before commitment, and keeping context visible across multi-step flows.

**Confirmation policy:** undo by default; confirmation reserved for the irreversible; escalated friction (re-entering a password) only for the catastrophic. Blanket confirmation dialogs produce habituation and protect nobody.

## Microcopy

Microcopy is **fewer than three sentences**. Classify each piece by **one** primary goal — Inform, Influence, or Interact — chosen from purpose and context, not from the element it sits in. `(qualitative)`

Button labels are *interaction* microcopy: state what happens on click. The most common failure is one label trying to do all three jobs at once — informing, persuading and instructing in four words.

For links and buttons: **Specific, Sincere, Substantial, Succinct**, in that priority order. Specificity beats brevity when they conflict, because eyetracking shows users read links without the surrounding text — the label must work in isolation. There is no word limit; an 11-word link has outperformed a vague two-word one. Clarify ambiguous actions ("Continue to payment"); do not decorate already-clear ones.

## Timing budget

- Visual feedback for any interaction: **0.1 s**.
- Hover-triggered content: **0.3–0.5 s** delay before showing, so passing the cursor over doesn't flash content.
- Slider feedback: under **0.1 s**, or the control feels broken.
- Anything above ~1 s needs a progress indicator; above ~10 s it needs a way to leave and come back.
