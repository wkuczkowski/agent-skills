# Architecture patterns

[Handbook index](index.md) · Research reviewed: 2026-09-21

These patterns are design options, not a mandatory pipeline. Choose the smallest combination that answers the current task. Question examples in the use-case catalog are conceptual specifications; turn them into the current SDK contract only after selecting an integration.

## Pattern map

| Pattern | Shape | Representative cases |
|---|---|---|
| Semantic branch | Observe → judge → route in code | [Handler routing](use-cases/agents-and-routing.md#uc-ag-01), [support](use-cases/operations-and-support.md#uc-op-01) |
| Speculative fan-out | One state → independent questions → consume the relevant branch | [Bounded tools](use-cases/agents-and-routing.md#uc-ag-02), [home control](use-cases/interactive-systems.md#uc-in-02) |
| Candidate selection | Parse or retrieve → assess candidates → resolve IDs | [Value extraction](use-cases/data-and-documents.md#uc-dd-01), [reranking](use-cases/search-and-knowledge.md#uc-sr-01) |
| Evidence verification | Candidate claim or field + source → relationship check → review or retry | [Citations](use-cases/search-and-knowledge.md#uc-sr-04), [extraction cascade](use-cases/agents-and-routing.md#uc-ag-05) |
| Reusable dimensions | Assess each dimension → store → combine later in code | [Product matching](use-cases/commerce-and-content.md#uc-cc-02), [editorial review](use-cases/commerce-and-content.md#uc-cc-05) |
| Hierarchical search | Judge current frontier → fetch narrower evidence → judge again | [Taxonomy](use-cases/data-and-documents.md#uc-dd-05), [repository navigation](use-cases/software-and-security.md#uc-sw-02) |
| Stateful control | Snapshot → bounded decision → validate freshness → act → observe | [Mobile](use-cases/interactive-systems.md#uc-in-01), [games](use-cases/interactive-systems.md#uc-in-04) |
| Text-to-features | Versioned judgments → numeric features → separate supervised model | [Feature discovery](use-cases/research-and-analytics.md#uc-ra-04) |

## Shared-state batching is not dependent reasoning

Ask multiple useful questions together when each can be answered from the same input. For a device command, target room and requested operation can be assessed independently. A speculative branch question must state its premise: “Assuming this is a lighting request...” Code ignores that answer for non-lighting requests. The model does not first read another answer in the same request. See [fan-out](https://docs.typesafe.ai/patterns/fan-out).

A second request is justified when the first result determines which document to fetch, which full skill to inspect or which candidate set now exists. A request-per-step design is wasteful only when those steps did not actually depend on new evidence. Measure the tradeoff; an enormous speculative question set also consumes budget.

Batching questions about one shared state and issuing concurrent requests for separate records are different optimizations. Grouping unrelated customers into one state may increase exposure, create confusion and complicate isolation. The [parallel-questions cookbook](https://docs.typesafe.ai/cookbooks/parallel_questions) is a worked example, not a promise that every workload has its reported savings.

## Candidate-first systems have two quality bottlenecks

Candidate generation determines what can be selected. Semantic assessment determines how well eligible candidates are distinguished. Test both. A perfect selector over an incomplete shortlist still misses the answer.

Keep stable IDs and source offsets. In a large corpus, first retrieve or parse a manageable candidate set, then assess it. When ranking separate candidates, use the same question and rubric. When selecting one item from a set, remember the distribution is conditional on that set. See [reranking](https://docs.typesafe.ai/cookbooks/rerank_typesafe) and [pre-parsed extraction](https://docs.typesafe.ai/cookbooks/pre_parsed_value_extraction_cookbook).

## Verifiers produce evidence signals, not proof

A verifier should receive the original evidence as well as the proposed output. Asking whether a plausible answer “looks right” is weaker than checking a specific field against its source. Keep exact checks, such as quote matching and schema validation, before semantic checks.

A retry loop needs a termination policy. It should not keep sampling until a verifier approves. Correlated model mistakes and missing evidence can survive multiple passes. Record which field failed, what evidence changed and which component owns the final decision. The [SDE cascade](https://docs.typesafe.ai/cookbooks/sde_cascade) supplies an implementation example, not a general guarantee of large-model quality.

## Separate compensating preferences from mandatory constraints

A weighted combination is useful when one preference may legitimately compensate for another. First define score direction, normalization, missing-value handling and the meaning of the weights. Store raw dimensions so different users or policies can reweight without rerunning unchanged judgments.

Mandatory conditions need explicit gates. A source-access failure, unsupported required feature or missing authorization should not disappear into an average. If the evidence, model, criteria or task scope changes, cached judgments may no longer be reusable. See [composite scoring](https://docs.typesafe.ai/patterns/composite-scoring).

## Preserve uncertainty through hierarchies

A broader answer can be more useful than a false-specific leaf. However, an uncertain leaf does not establish that its parent is correct. Preserve alternatives across branches where needed and check support at the level actually reported. Aggregate path values only as documented heuristics until separately evaluated. See [hierarchical classification](https://docs.typesafe.ai/cookbooks/hierarchical_classification) and [coarse classification](https://docs.typesafe.ai/cookbooks/classification_using_confidence).

## Keep interactive actions coupled to observations

Recommended control-loop invariants: action candidates belong to one snapshot; the chosen target must still exist; known preconditions must still hold; the actor must be authorized; completion is checked from a new observation. Define waiting, blocked, stale-result and partial-success states explicitly.

An inference retry and an action retry have different consequences. After a tool timeout, the action may already have happened. Reconcile with the system of record before repeating an externally visible mutation. The [mobile-jev README](https://github.com/droidrun/mobile-jev) is a useful external design reference, not a runtime guarantee.

## Build an explicit failure envelope

Recommended application states are `accepted`, `review_required`, `no_match`, `missing_evidence`, `invalid_request`, `service_error`, `stale_observation` and `action_failed`. These names are **proposed application states, not native API response types**.

A provider error is not a negative semantic answer. A low-confidence unused branch is not necessarily a failed request. A confident permitted action can still fail at execution. Keeping these states separate makes telemetry and recovery meaningful. The [decision template](templates/decision-spec-template.md) captures the policy in one place.
