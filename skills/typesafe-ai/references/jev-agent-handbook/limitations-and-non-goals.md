# Limitations, non-goals and failure diagnosis

[Handbook index](index.md) · Research reviewed: 2026-09-21

This handbook describes useful design hypotheses and documented integration patterns. It does not certify Jev, the linked projects or any proposed workflow for a particular domain.

## Capability boundaries that must be checked live

Jev’s documented interface is for typed judgments rather than producing a new narrative answer. Use an appropriate generator when new prose, code or arbitrary content is required. Extractive output remains possible when another component enumerates candidates and copies the selected source material. See the [introduction](https://docs.typesafe.ai/introduction) and [value-extraction cookbook](https://docs.typesafe.ai/cookbooks/pre_parsed_value_extraction_cookbook).

The inspected model/state documentation described text-based inputs. Do not assume direct image, audio, video or screenshot understanding from an example whose surrounding system supplies extracted text or a UI tree. Verify supported modalities and language performance through the current [models](https://docs.typesafe.ai/models) and [state](https://docs.typesafe.ai/concepts/state) documentation. A future release may change these capabilities.

Do not treat the model as a web browser, database, clock, calculator, authorization server or tool executor. Supply current facts from tools and keep exact operations in code. The [version-specific jaggedness report](https://docs.typesafe.ai/model-jaggedness/jev-1.13) documents weaknesses including exact calculations, some contextual reasoning and adversarial influence. These observations justify tests; they are not claims about every future model version.

## Interface correctness is not semantic correctness

| Apparent success | What can still be wrong |
|---|---|
| The output is a valid category. | Every category was unsuitable, or the selected category is wrong. |
| A selected value is copied exactly. | It belongs to the wrong person, time period or field role. |
| The score has the expected numeric range. | The rubric is ambiguous or its expected position hides a split distribution. |
| The model returns high confidence. | It confidently follows misleading evidence or the wrong task interpretation. |
| Two components agree. | They share the same missing evidence or failure mode. |
| A citation exists. | The source context does not support the claim or is outdated. |
| A permitted tool was chosen. | Its target, arguments, timing or user authorization is wrong. |
| A UI action was sent successfully. | The requested real-world state was not achieved. |

These distinctions are why [evaluation](evaluation-and-operations.md) separates evidence, judgment, policy and execution.

## Sources of failure to diagnose separately

**Evidence failure:** missing documents, wrong retrieval scope, low parser recall, incomplete context, stale observations or lost qualifications. Adding another model may not recover absent evidence.

**Specification failure:** overlapping labels, missing no-match outcomes, contradictory instructions, unclear score levels, hidden requirements in IDs or undefined policy. Fix the decision contract before tuning thresholds.

**Model error:** incorrect semantic interpretation despite adequate evidence and a clear task. Record representative counterexamples and evaluate an alternative formulation, model or non-model method.

**Composition failure:** consuming a speculative answer on the wrong branch, treating correlated probabilities as independent, flattening mandatory gates into an average or overconfidently promoting a leaf to its parent.

**Execution failure:** invalid arguments, stale target IDs, lost permissions, an API timeout or a duplicated side effect. A correct semantic answer cannot prevent these without ordinary systems engineering.

**Evaluation failure:** leaked labels, cherry-picked examples, reusing the test set during prompt tuning, unrepresentative review samples or comparing live traffic with cached demonstration results.

## No implicit security or privacy guarantees

A typed decision can still be influenced by malicious source text. A guardrail model is not an authorization boundary, and a community connector is not safe merely because its source is public. Keep tool scopes and sensitive operations controlled independently. [OWASP’s guidance](https://cheatsheetseries.owasp.org/cheatsheets/LLM_Prompt_Injection_Prevention_Cheat_Sheet.html) provides general design context.

Data routing depends on the actual contract and deployment path. Verify permitted data categories, retention, training use, processing region, subprocessors, gateway logs and deletion behavior through current agreements. The [TypeSafe legal index](https://docs.typesafe.ai/legal) is only a starting point. This package makes no blanket claim about zero retention, local processing or legal compliance.

## Consequential domains remain review workflows

The [specialist use cases](use-cases/specialist-review.md) prepare evidence and identify missing information. They do not authorize autonomous hiring decisions, clinical decisions, claim denial, financial restrictions or legal conclusions. Required professional review and applicable rules must be established for the actual organization and jurisdiction.

## When not to add Jev

Do not add a model to an exact lookup, a calculation, an already-known UI intent or a workload whose current deterministic solution is adequate. Do not add an inference gate when there is no meaningful uncertainty policy or no capacity to review the cases it produces. Do not use a classifier as a shortcut around missing evidence or permission.

These are design recommendations, not a statement that one model class is universally unsuitable for a broad field. Evaluate the specific bounded task and its consequences.
