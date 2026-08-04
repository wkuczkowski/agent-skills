# Adaptive screens, recommendations and post-purchase

## Adapting to the user's stage

The idea: a new user, a returning user and a power user should not get the same home screen.

- **New** — keep it simple. Welcome, one goal-setting action, a few easy entry points. Enough to explore, not enough to overwhelm.
- **Returning** — skip the onboarding furniture; lead with the actionable thing for today.
- **Power** — lead with status and stats, then the plan, then suggestions. Help them optimise, not get started.

This is sound as a design instinct and `(untested)` as a conversion claim. It also has documented costs that the usual telling of it leaves out:

**Adaptive interfaces break spatial memory.** A user who learned where something was and finds it moved has been punished for becoming experienced. If the layout changes by stage, keep anchors stable — navigation, primary action position, and the route to anything the user has already used stays put; only content within the frame adapts.

**Stage detection is usually wrong at the edges.** Someone returning after six months is not a power user, and a power user on a new device is not a beginner. Decide what happens when the signal is ambiguous, and make the richer state reachable manually — a "show me everything" route out of the simplified version.

**Never make a stage change feel like a downgrade.** Removing a feature someone used because they now look like a different segment reads as a bug.

## Recommendations and personalization

Personalization backfires in five documented ways: `(qualitative)`

1. **Content fatigue** — the same recommendations circulating until the user stops seeing them.
2. **Narrow interest segmentation** — being reduced to one behaviour. The complaint NN/g records is "I'm not defined by cats, I'm this other person too."
3. **Echo-chamber reinforcement** — the model narrowing what the user is shown until nothing new appears.
4. **Creepiness** — most acutely from cross-device tracking the user did not know about.
5. **Stale history** — recommendations built on a gift purchase from two years ago.

Requirements for any recommendation UI:

- **State the data source.** "Because you watched X" — the explanation is the difference between helpful and unsettling.
- **Separate recommendations into labelled categories** rather than one undifferentiated feed.
- **Let users fine-tune**: rate items, hide a recommendation, remove an item from history. The gift-purchase case alone justifies this.
- **Update visibly and quickly after feedback**, so the system is seen to learn. Feedback that changes nothing observable teaches users to stop giving it.

## Order tracking and post-purchase

The gap between payment and delivery is full of uncertainty, and a screen that dumps order number, item list and a column of dates makes the user do the interpreting.

**Order tracking is the single most-wanted account feature — 51% of US shoppers**, ahead of managing payment methods (30%), order history (26%) and loyalty programs (24%). `(measured)`

What the screen should do: `(qualitative)`

- **Lead with resolved status in plain language** — "Arriving Thursday" beats "In transit". Pair it with the delivery window and address.
- **Show a visual timeline** rather than a block of dated text, so the current stage is legible at a glance.
- **Humanise the handoff** where a person is involved — name, and a direct route to contact them. This is trust and responsiveness, not decoration.
- **Answer the next question before it is asked**: what happens if nobody is home, how to change the address, how to return it.

A caution on the courier photo: it is a real person's likeness. Show it only where you have the right to, and never fabricate one for a mockup.

## The confirmation page

Underused. It is the one moment when the user has already succeeded and has spare attention. `(measured)`

- **Offer account creation here**, not before — the data is already entered, so it is one click. 42–54% of sites don't defer it, and **57% fail to state concrete account benefits**. Vague benefit copy performs nearly as poorly as none.
- Relevant cross-sells with an "Add to order" action while the order is still amendable.
- Delivery details, setup or usage guidance, and the return window.
- A low-effort survey, if you will act on it.

**48% of newsletter unsubscribes are driven by a single retailer emailing too frequently.** Whatever you sign the user up for here, the frequency is the part that loses them.

## Back-button behaviour

**59% of sites violate Back-button expectations.** Users treat Back as undo-navigation, and breaking it — trapping them in a flow, skipping over states, or losing entered data — is one of the fastest ways to lose a session. Test the Back button explicitly at every step of any multi-step flow. `(measured)`
