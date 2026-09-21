# Decision design

[Handbook index](index.md) · Research reviewed: 2026-09-21

This page is a design-review aid, not a replacement for the existing TypeSafe skill or the live API contract. Its checklists and example policies are handbook recommendations. Start with the smallest useful semantic judgment in the actual application.

## Define the observable outcome

Write down what the application should **show, select, record or hand to someone else**. Then identify the part ordinary code cannot reliably infer from the available text. “Use AI to process invoices” is too vague; “choose which existing amount span is the outstanding balance, preserving provenance” is a testable decision.

Compare at least the relevant alternatives: exact rules or parsers, search plus manual selection, a conventional classifier, a generative model, and a Jev decision component. Prefer ordinary code when the intended behavior is already exact. Prefer a generative component when the deliverable itself is new prose, code or an unconstrained structure. A hybrid is often the appropriate experiment, not an automatic recommendation.

Record the cost of a wrong action, the reversibility of the action and what happens when evidence is missing. Use the [decision specification template](templates/decision-spec-template.md) before implementing a consequential workflow.

## Choose by semantics, not by output aesthetics

| Intended question | Starting primitive | Do not confuse it with |
|---|---|---|
| Which one of the available alternatives applies? | [Choice](https://docs.typesafe.ai/primitives/choice) | Independent confidence that every candidate is suitable. |
| Is a precisely described proposition supported? | [Noul](https://docs.typesafe.ai/primitives/noul) | How severe, intense or desirable something is. |
| To what degree does an item satisfy one described dimension? | [Score](https://docs.typesafe.ai/primitives/score) | An exact measurement, an extracted number or an automatic probability of success. |
| Which of several labels apply simultaneously? | Usually one Noul per label, possibly with a separate evidence-status question. | A single mutually exclusive Choice. |
| Which source text should be returned? | Candidate generation plus Choice or ranking, then verbatim copying in code. | Text generation by Jev. |

A question may concern a contextual relationship, such as whether a source supports a claim. “Narrow” does not mean stripping away the relationship and asking disconnected keyword questions. Split dimensions when their answers are independently useful; keep joint meaning when decomposition would lose the decision.

## Make evidence and uncertainty representable

Use an explicit distinction between **present, absent, not established and conflicting** whenever the application needs it. “The document does not state a delivery date” is not “delivery will never occur.” A low probability of a proposition also does not, by itself, explain whether the model found contrary evidence or simply lacked context.

For extractive selection, include a no-match path and evaluate the candidate generator’s recall. For harmless preference selection, several equally acceptable options may not require escalation. For an irreversible action, ambiguity is consequential. The policy should depend on the actual action, not a universal confidence number.

## Specify state deliberately

A recommended state envelope separates the following roles. These are application design fields, **not required TypeSafe API keys**.

| Role | Suggested content |
|---|---|
| Task | The current request, desired outcome and relevant conversational antecedents. |
| Evidence | Original text, source identifiers, offsets, extraction quality and document versions. |
| Policy | The applicable definitions, rubric and policy version, from a trusted source. |
| Candidates | Stable IDs, descriptions, provenance and eligibility already established in code. |
| Context | Relevant identity relationships, locale, reference time and business scope. |
| Observation state | Snapshot ID, observed time and facts verified by tools. |
| Prior hypotheses | Explicitly labeled earlier inferences, never silently promoted to observations. |

Use meaningful field names and reference the relevant paths in the question. Exclude irrelevant personal data, secrets and large unrelated logs. Preserve enough surrounding text to interpret negation, qualifications and who did what to whom. Follow the current [state guidance](https://docs.typesafe.ai/concepts/state) for accepted input representations.

## Design instructions and criteria together

The question must contain its complete semantic meaning. Question IDs are application handles, not a place to hide instructions. This distinction is documented in the [primitives guide](https://docs.typesafe.ai/primitives).

Define alternatives with contrasts and exclusions. For example, “a request for a receipt” should not compete ambiguously with an overbroad “anything involving money” label. Score levels should describe independent observable situations on one dimension, not simply “bad / okay / good.” Structured criteria can help when short strings become unclear; consult [advanced structures](https://docs.typesafe.ai/primitives/advanced) before encoding them.

Use evidence selection to make a result inspectable. A selected source span is useful provenance, but is not access to the model’s internal explanation and does not prove the semantic judgment correct.

## Interpret the response at the right level

For a native Score with ordered zero-based levels, the reported value is an expected level position. With three levels, a middle value can result from probability concentrated in the middle or from disagreement between the two extremes. Preserve the distribution when that distinction affects routing. Normalizing a score for a weighted dashboard is a design choice, not proof the underlying rubric has equal real-world intervals. See [Score](https://docs.typesafe.ai/primitives/score).

Choice and Score concentration, the probability of a selected option, the correctness of a prediction and permission to act are different concepts. A Noul near the middle expresses uncertainty about its proposition, not moderate intensity. Check the current [confidence](https://docs.typesafe.ai/confidence) and [Noul](https://docs.typesafe.ai/primitives/noul) definitions before writing thresholds.

Do not multiply separate judgment probabilities and call the result a calibrated joint probability without validating the dependence assumptions. Independent execution of questions is not statistical independence of the facts being judged.

## Finish with a deterministic policy

Define how code combines raw judgments, what invalidates a result, which constraints cannot be traded away and who may authorize a side effect. A high preference score must not offset an access-control failure. Version the policy separately from questions so changing a review threshold does not require redefining the semantic task.

Before release, evaluate actual downstream outcomes using [evaluation and operations](evaluation-and-operations.md). A well-formed response is only the start of the check.
