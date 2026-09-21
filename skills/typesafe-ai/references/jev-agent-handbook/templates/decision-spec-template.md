# Decision specification template

[Handbook index](../index.md) · [Decision design](../decision-design.md) · [Evaluation](../evaluation-and-operations.md)

Use this template to turn a use-case idea into a reviewable implementation plan. These are application specification fields, not the TypeSafe wire schema. Fill with actual evidence and explicit assumptions; unsupported entries remain unknown rather than guessed.

## Decision contract

| Field | What to record |
|---|---|
| Decision ID and owner | Stable application identifier, responsible engineer and policy owner. |
| User-visible outcome | What the application will show, select, record, execute or hand off. |
| Current baseline | Existing method and the specific problem this experiment is intended to improve. |
| Scope and exclusions | Supported inputs, users, languages, environments and out-of-scope cases. |
| Semantic judgment | The precise relationship, category, proposition or dimension to assess. |
| State sources | Trusted facts, untrusted content, source versions, identity scope and freshness. |
| Candidate construction | Parser/retriever, stable IDs, measured coverage and no-match behavior. |
| Questions | Complete instructions, selected primitive and fully defined criteria. |
| Dependency topology | Shared-state independent questions, speculative premises and necessary later stages. |
| Interpretation | Meaning of each response and what must not be inferred from it. |
| Hard invariants | Exact validation, authorization, tenancy, arithmetic and safety rules owned by code. |
| Review policy | Risk-dependent thresholds, missing-evidence behavior, human owner and queue capacity. |
| Action policy | Allowed side effects, confirmations, idempotency, stale-state checks and rollback. |
| Error policy | Invalid request, timeout, rate limit, missing answer, partial processing and reconciliation. |
| Data handling | Minimum necessary data, approved provider route, secrets, logging and retention. |
| Version record | Model, adapter, SDK, questions, taxonomy, parser and policy versions. |

## Evaluation contract

| Field | What to record |
|---|---|
| Reference outcomes | Independent labels, source spans, tool results or observed state transitions. |
| Dataset | Sampling frame, prevalence, slices, exclusions and reviewer disagreement. |
| Split discipline | Training/discovery, validation/calibration and untouched test boundaries. |
| Baseline comparison | Deterministic, existing human or other model workflow under comparable conditions. |
| Primary error | The wrong result or action whose cost matters most. |
| Acceptance criteria | Pre-agreed quality, coverage, review load, latency and total-cost conditions. |
| Stress tests | Missing candidates, negation, adversarial text, stale facts, multiple valid answers and service failure. |
| Monitoring | Random audits, drift indicators, incidents, reviewed corrections and rollback triggers. |
| Recheck triggers | Model, policy, source, parser, SDK or workflow changes that invalidate prior evidence. |

## Compact worked specification excerpt

**Decision:** select the current invoice-recipient email from a document. **Input:** source text and parser-generated email spans with offsets. **Question:** Choice over candidates, with not-stated and ambiguous outcomes, explicitly asking for the address designated for future invoices. **Code:** copy the exact span, validate syntax, preserve history and check permission before a database update. **Uncertainty:** queue ambiguous roles; do not substitute a guessed address. **Evaluation:** field accuracy and candidate recall, with historical-contact and forwarding-header cases. **Excluded claim:** finding a valid email does not prove it is the intended recipient or authorize sending a message.

See [UC-DD-01](../use-cases/data-and-documents.md#uc-dd-01) for the expanded example.
