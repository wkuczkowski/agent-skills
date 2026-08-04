# Paywalls, trials and upgrade prompts

## Ask the easy question

A classic paywall makes the brain ask *"Is this worth $19/month?"* — a brutal question for someone who opened the app 30 seconds ago and has felt no value. Reframing to *"Can I try this for free?"* asks a question whose answer is obviously yes.

The reframing mechanism is real: framing reversals of this size replicate (Tversky & Kahneman's Asian-disease problem reproduced in 31 of 36 samples in Many Labs 1), and zero-price effects are large (Shampanier, Mazar & Ariely 2007: demand rose from 27% to 69% at $0.00 vs $0.01). `(mechanism)`

**But "trial-forward always wins" is false.** Free-trial-acquired customers showed **59% lower lifetime value** than directly acquired ones in the one peer-reviewed study available (Datta et al. 2015, *JMR* 52(2)). Pick the structure from your economics, not from a screenshot.

## Structure before screen

Large-N benchmarks describe monetisation *structures*, and they are worth using for that. They measure **nothing** about headlines, timelines, imagery or button copy. `(measured, observational)`

- Hard paywall: **10.7% install-to-paid within 35 days** vs 2.1% for freemium — note the denominator is *installs*, not trial starts. Anyone told 10.7% is a trial-to-paid rate will set wildly wrong expectations.
- **$3.09 vs $0.38 revenue per install at Day 60**; 12-month retention essentially identical (27% vs 28%).
- Trials of **17–32 days convert at 42.5% trial-to-paid vs 25.5% for trials under four days**.
- **55.4% of 3-day-trial cancellations happen on Day 0**; 84% within Days 0–1. If your trial is three days, the decision is made immediately — the trial is not doing the work you think it is.
- Hard paywalls show ~21% higher LTV per subscriber in a second vendor dataset.

Both datasets confound strategy with app quality: a well-differentiated app can *afford* a hard paywall. Use them to choose a structure; use your own adequately-powered experiments for anything about the screen — and read `evidence.md` before assuming you can run one.

## The trial timeline

Replace a feature list with a timeline that volunteers the charge date:

```
Today   → Full access to everything
Day 5   → We email you a reminder before the trial ends
Day 7   → First charge, £9/month. Cancel any time before.
```

Why this is worth doing, stated honestly: `(untested for conversion, defensible downstream)`

- No controlled published experiment isolates a trial-timeline component. The one adjacent case study is a 2007 B2B test of a pre-checked reminder-email checkbox — not a mobile paywall.
- The **defensible** benefit is downstream: fewer refunds, fewer chargebacks, fewer one-star reviews. That is worth having on its own.
- The mechanism is not "transparency bias" — that construct does not exist. What you are doing is reducing perceived commitment risk, and disclosing terms you are in many cases **required** to disclose (see `bright-lines.md` L15, P1).

**Platform reality:** Apple sends no guaranteed trial-end reminder — if you promise one on iOS, you must build and send it yourself. Google does email users before a trial ends. A promise you do not keep is a misrepresentation.

## Copy

- **Keep the primary button literal.** "Start free trial" is fine because it is accurate, not because "start" is psychologically lighter than "subscribe" — that claim comes from a single unpublished 2013 test whose own author reported it failed in another language. Do not build a copy system on it.
- **Kill uncertainty with facts, not adjectives.** "No card required" and "Cancel in Settings, two taps" answer real questions. "Quick setup" answers none. Precision reads as credible — but only when the claim is attributable and true. `(mechanism)`
- **Show the real product.** A decorative hero does not answer "what am I getting?". Show actual screens, actual content, actual output. Users cannot commit to something they cannot visualise. `(untested for conversion, strong on comprehension)`

## Upgrade and win-back prompts

**Loss framing is a variant to test, not a rule.** The 2× loss coefficient is contested down to roughly 1.0 in the largest reanalysis. Goal framing — the type that applies here — is the weakest and least consistent of the three framing types.

Where you use it:

- Name the **real** consequence with real specifics: the actual item count, the actual date, from live data.
- **Never fabricate the loss.** No invented deadlines, no "your data will be deleted" if it will not be. Beyond the ethics, a countdown to a fake event is a per-se banned practice in the EU and UK (`bright-lines.md` L11).
- **Never shame the decline.** "Not now" — not "I'll risk it". That is confirmshaming, named in the FTC taxonomy and Colorado rules (L9).
- Consider whether a real deletion policy is one you want at all: a genuine countdown means your product destroys user data on a deadline, which carries its own trust cost.

**Do not attribute any of this to status quo bias.** Status quo bias predicts the user dismisses your prompt and keeps their current plan — it is the force you are working against.

## Nagging

Modal interruptions are the most-hated technique measured on both desktop and mobile (452 US adults, average dislike 5.23 of 7, mobile significantly worse, p < 0.0001). The discriminating variable is **forced dismissal** — formats requiring an action scored worst; formats leaving control with the user scored best. This applies directly to upsell interstitials, newsletter modals and exit-intent popups. `(measured)`

Re-prompting a user who already declined, within the same visit, is a rule violation in Colorado and under the DSA (L12).

## Checklist

- [ ] The screen asks an easy question, and the hard one is answered somewhere on it
- [ ] Charge date, amount and cancellation route are stated before any billing field
- [ ] Any reminder you promise is one you have actually built
- [ ] Hero shows the real product
- [ ] Every number on the screen comes from live data
- [ ] Dismiss label is neutral; declining is one tap and is remembered
- [ ] Cancelling takes no more steps than subscribing
