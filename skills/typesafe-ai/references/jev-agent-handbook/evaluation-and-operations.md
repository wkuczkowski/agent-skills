# Evaluation and operations

[Handbook index](index.md) · Research reviewed: 2026-09-21

The following is a proposed engineering protocol, not a claim that the use cases in this package have passed it. No live TypeSafe API experiments, benchmark reproductions or installations were performed while preparing this handbook.

## Evaluate the application, not just the label

Define a reference outcome independent of the model: a human-adjudicated category, a source span, a verified function invocation, an engine-legal move or a task completed in the actual system. For ambiguous language, retain reviewer disagreement rather than forcing an artificial single truth.

Measure distinct layers: whether the input contained the needed evidence; whether candidate generation included the right answer; whether the model judged correctly; whether code interpreted it correctly; whether the action was authorized; and whether execution achieved the intended state. Otherwise an improved classifier may appear responsible for a parser or tool failure.

The official [Noul consistency](https://docs.typesafe.ai/cookbooks/consistency_noul_cookbook) and [Choice consistency](https://docs.typesafe.ai/cookbooks/consistency_choice_cookbook) examples are useful reminders to test repeated fixed inputs. Repeatability is not accuracy: a consistently wrong prediction remains wrong.

## Build a representative evaluation set

Include ordinary cases, meaningful rare cases and adversarial cases. Recommended slices include language, input source, length, extraction quality, category, missing evidence, multiple valid alternatives, negation, time-sensitive facts and policy version. Preserve representative traffic prevalence for deployment estimates; keep targeted stress tests as a separately reported collection.

Separate training or feature-discovery data, validation data for prompt and threshold selection, and an untouched test set. Prevent duplicate documents, versions of the same record or the same entity from leaking across splits when that would make the test unrealistically easy. Use chronological evaluation when future distribution matters. Synthetic cases help expose specified failure modes but do not establish real-world accuracy on their own.

Benchmark the actual alternatives: deterministic rules, the existing workflow, a generative structured-output model or an available supervised classifier. A simple task may not justify an extra service call.

## Match metrics to the decision

| Workflow | Primary checks | Frequently misleading shortcut |
|---|---|---|
| Single-label classification | Confusion by class, errors under the chosen policy, ambiguous-case handling. | Overall accuracy hides a costly minority-class failure. |
| Multi-label coding | Per-label precision/recall, contradictory labels and missing evidence. | Treating non-selection as explicit absence. |
| Retrieval and ranking | Candidate recall, graded ranking quality and final answer success. | Attributing a missing candidate to the reranker. |
| Extraction | Field accuracy, provenance, omitted-field recall and wrong-role selection. | Valid JSON or a valid-looking email address. |
| Verification | False acceptance of unsupported output and unnecessary rejection. | Agreement with the generator. |
| Routing or cascades | End-to-end quality, total cost, review burden and wrong-handler consequences. | Cost per Jev request alone. |
| Interactive actions | Verified task completion, stale actions, unintended effects and recovery. | The model selecting `DONE`. |
| Specialist support | Missed relevant evidence, false assertions and reviewer corrections. | A model’s overall verdict about a person or case. |

## Thresholds and calibration

Define separate policies where errors have different consequences. A low-risk display preference and an external data transfer should not share one threshold. Review a coverage-versus-error curve: among cases accepted automatically, how often was the action wrong, and how much traffic was sent to review?

For a yes/no task, compare predicted probability ranges with observed positive frequencies on suitable labeled data. If recalibration is needed, fit it on held-out calibration/validation data, not the final test set. Probability-quality metrics such as Brier loss or log loss are useful but do not isolate calibration by themselves; inspect reliability diagrams and sample counts. The [scikit-learn calibration documentation](https://scikit-learn.org/stable/modules/calibration.html) explains these distinctions. It is a methods reference, not evidence of Jev’s local performance.

Do not read native Choice/Score confidence as a calibrated probability that the full workflow is correct. Review the current [TypeSafe confidence definition](https://docs.typesafe.ai/confidence). For a binary decision, an application may define lower and upper action bounds with review between them, but this handbook provides no universal numerical values.

## Test uncertainty and invariants explicitly

Include no-match inputs, conflicting evidence, two equally valid alternatives and missing required state. Test option-order changes and paraphrases when the output should remain invariant. Test a changed fact that should change the decision as well; stability is not always desirable.

If symmetry, exclusivity, transitivity or a complement relationship is required, enforce the necessary property in code or validate it. Do not assume independent semantic judgments will satisfy a formal invariant. Keep raw distributions when their shape matters, rather than rounding an expected Score into a category without analysis.

For a verifier, include persuasive but unsupported claims. For a guard, include malicious instructions in data as well as harmless quotations of instructions. For a classifier, include out-of-taxonomy inputs. Consult the current model’s limitations through the [models page](https://docs.typesafe.ai/models); the linked [Jev 1.13 report](https://docs.typesafe.ai/model-jaggedness/jev-1.13) is version-specific evidence, not a permanent capability boundary.

## Measure real performance and cost

Measure end-to-end median and tail latency, not just inference time. Include candidate retrieval, serialization, network calls, concurrency limits, retries, review and downstream generation. Record the actual provider route, model identifier, SDK version and workload size. Distinguish shared-state batching from concurrent calls with separate states.

Separate live calls from cached/replayed results. A notebook with cached answers can demonstrate analysis logic without reproducing the claimed live speed or cost. A scripted animation is not an API measurement. Do not compare benchmark numbers from different candidate sets, hardware, model versions or task interfaces as though the conditions were identical.

## Production control and observability

Recommended structured records include request ID, sanitized evidence reference, state hash or snapshot ID, question and rubric versions, model and adapter versions, raw judgment, policy version, selected branch, review outcome and observed action result. Retain only what is justified; logs containing source documents are a separate sensitive-data store, not a harmless debugging detail.

Use resource-specific authorization, schema validation and least privilege outside the model. A semantic guard is only an additional signal. See [OWASP prompt-injection prevention](https://cheatsheetseries.owasp.org/cheatsheets/LLM_Prompt_Injection_Prevention_Cheat_Sheet.html) for general controls; that guidance is not a Jev security certification.

Define behavior for timeouts, rate limits, invalid requests, missing answer IDs and partial processing. An inference retry policy does not make repeating a payment, email or device action safe. Keep idempotency and reconciliation in the execution layer. Consult the current [SDK retry reference](https://docs.typesafe.ai/sdk/python/api/retries) for transport behavior.

## Release and change management

Recommended rollout: deterministic contract tests, labeled offline evaluation, shadow mode without side effects, then a limited rollout with review and rollback. Agree acceptance criteria before seeing the final test results. A human review path needs an owner and capacity; it is not a magic fallback if no one receives the queue.

Reevaluate when a model, adapter, prompt, taxonomy, parser, policy or source distribution changes. Pin reproducible versions where supported, and verify what any moving model alias resolved to. A threshold change can alter error rates even with identical predictions. Maintain a representative random audit of accepted cases so confident mistakes are observable.

Use [maintenance](maintenance.md) for documentation updates and the [decision specification template](templates/decision-spec-template.md) for a deployable record of assumptions.
