# Use-case index

[Handbook index](../index.md) · [Architecture patterns](../architecture-patterns.md) · [Decision template](../templates/decision-spec-template.md)

**54 detailed cards across 9 areas.** This is a design and discovery catalog for agents, not a set of benchmarked production recipes. Each card includes a concrete input, conceptual questions, downstream behavior, limitations, evaluation criteria and sources. All example inputs are hypothetical.

## Evidence labels

| Label | Meaning | Cards |
|---|---|---:|
| W | An official worked example supports the underlying pattern. Local scenarios and proposed policies are adaptations, not measured reproductions. | 14 |
| E | A first-party external project, integration or experiment was inspected. No runtime reproduction or security audit was performed. | 6 |
| P | An original design proposal. Sources establish relevant primitives or related ideas, not application-specific effectiveness. | 34 |

These labels describe provenance, **not a quality ranking**. A documented demo can be unsuitable for production, while a proposed workflow can be worth testing. None of the labels means validated on your data. The [source registry](../sources.md) also distinguishes unread index-listed pages and fetch failures.

## Choose a starting area

| Area | Scope |
|---|---|
| [Agents and routing](agents-and-routing.md) | Handler selection, agent harnesses, escalation and context management. |
| [Search and knowledge](search-and-knowledge.md) | Retrieval, evidence selection, citations and reuse of existing answers. |
| [Data and documents](data-and-documents.md) | Extractive transformations, entity linking and taxonomies. |
| [Operations and support](operations-and-support.md) | Work queues, tickets, commitments and exception handling. |
| [Commerce and content](commerce-and-content.md) | Catalogs, matching, editorial review and feedback. |
| [Software and security](software-and-security.md) | Developer workflows, observability and bounded security checks. |
| [Research and analytics](research-and-analytics.md) | Screening, coding, semantic features and evaluation data. |
| [Interactive systems](interactive-systems.md) | Mobile actions, home controls, constrained UI, games and forms. |
| [Specialist review workflows](specialist-review.md) | Evidence preparation for qualified reviewers, not autonomous consequential decisions. |

## Cross-cutting entry points

| Need | Useful starting cases |
|---|---|
| Replace a prompt-and-parse decision | [Handler routing](agents-and-routing.md#uc-ag-01), [bounded arguments](agents-and-routing.md#uc-ag-02), [CLI decisions](software-and-security.md#uc-sw-06). |
| Return existing text instead of generating it | [Value spans](data-and-documents.md#uc-dd-01), [source search](search-and-knowledge.md#uc-sr-02), [structure recovery](data-and-documents.md#uc-dd-03). |
| Improve an agent harness | [Skill discovery](agents-and-routing.md#uc-ag-03), [model routing](agents-and-routing.md#uc-ag-04), [context pruning](agents-and-routing.md#uc-ag-06). |
| Ground and verify answers | [RAG triage](search-and-knowledge.md#uc-sr-03), [citations](search-and-knowledge.md#uc-sr-04), [extraction cascade](agents-and-routing.md#uc-ag-05). |
| Reuse judgments as data | [Editorial dimensions](commerce-and-content.md#uc-cc-05), [semantic features](research-and-analytics.md#uc-ra-04), [feedback coding](commerce-and-content.md#uc-cc-06). |
| Decide from evolving state | [Mobile](interactive-systems.md#uc-in-01), [home controls](interactive-systems.md#uc-in-02), [game moves](interactive-systems.md#uc-in-04). |
| Build UI without arbitrary generation | [Constrained composition](interactive-systems.md#uc-in-03), [next clarification](interactive-systems.md#uc-in-06). |
| Assist expert review | [Contract evidence](specialist-review.md#uc-sp-01), [claim packets](specialist-review.md#uc-sp-02), [clinical administration](specialist-review.md#uc-sp-04). |

## Complete card directory

| ID and case | Basis | Search terms |
|---|---|---|
| [UC-AG-01 — Route a request to an appropriate handler](agents-and-routing.md#uc-ag-01) | W | Choice; Noul; fan-out; handler-routing |
| [UC-AG-02 — Choose a known tool and bounded arguments](agents-and-routing.md#uc-ag-02) | W | Choice; tool-selection; bounded-arguments |
| [UC-AG-03 — Discover a relevant agent skill without loading the whole library](agents-and-routing.md#uc-ag-03) | P | Score; Choice; Noul; skill-selection; retrieval |
| [UC-AG-04 — Route work between a cheaper model, a stronger model and a human](agents-and-routing.md#uc-ag-04) | E | Score; Choice; Noul; model-routing; cascade |
| [UC-AG-05 — Verify inexpensive extraction before escalating](agents-and-routing.md#uc-ag-05) | W | Noul; Choice; verification; cascade |
| [UC-AG-06 — Prune tool history while preserving retained text](agents-and-routing.md#uc-ag-06) | E | Noul; memory; context-pruning; provenance |
| [UC-SR-01 — Rerank a retrieved shortlist](search-and-knowledge.md#uc-sr-01) | W | Score; Noul; reranking; retrieval |
| [UC-SR-02 — Find an exact supporting line or passage](search-and-knowledge.md#uc-sr-02) | W | Choice; Noul; semantic-find; extractive-search |
| [UC-SR-03 — Triage retrieved passages before answer generation](search-and-knowledge.md#uc-sr-03) | W | Noul; RAG; evidence-triage; guardrails |
| [UC-SR-04 — Check whether a citation supports a claim](search-and-knowledge.md#uc-sr-04) | W | Choice; Noul; citation-verification; grounding |
| [UC-SR-05 — Decide whether a cached answer is reusable](search-and-knowledge.md#uc-sr-05) | P | Choice; Noul; semantic-cache; freshness |
| [UC-SR-06 — Surface conflicting claims across sources](search-and-knowledge.md#uc-sr-06) | P | Choice; Noul; contradiction; knowledge-maintenance |
| [UC-DD-01 — Select a source value from parser-generated candidates](data-and-documents.md#uc-dd-01) | W | Choice; Noul; span-selection; extraction |
| [UC-DD-02 — Interpret date expressions and resolve them in code](data-and-documents.md#uc-dd-02) | W | Choice; temporal-interpretation; calendar-validation |
| [UC-DD-03 — Recover document structure without rewriting the text](data-and-documents.md#uc-dd-03) | W | Noul; Choice; extractive-transformation; formatting |
| [UC-DD-04 — Link candidate records that refer to the same entity](data-and-documents.md#uc-dd-04) | W | Score; Noul; entity-resolution; record-linkage |
| [UC-DD-05 — Classify into a hierarchy and stop at defensible specificity](data-and-documents.md#uc-dd-05) | W | Choice; Noul; hierarchy; coarse-fallback |
| [UC-DD-06 — Extract a relation by selecting evidence and a defined relation type](data-and-documents.md#uc-dd-06) | P | Choice; Noul; relation-extraction; knowledge-graph |
| [UC-OP-01 — Classify support intent, impact and the next queue](operations-and-support.md#uc-op-01) | P | Choice; Score; Noul; support-triage |
| [UC-OP-02 — Find duplicate tickets without erasing distinct incidents](operations-and-support.md#uc-op-02) | P | Choice; Noul; deduplication; incident-linkage |
| [UC-OP-03 — Recognize commitments and task candidates in meeting notes](operations-and-support.md#uc-op-03) | P | Choice; commitment-detection; task-drafts; extraction |
| [UC-OP-04 — Route invoice and purchase-order exceptions](operations-and-support.md#uc-op-04) | P | Choice; Noul; document-matching; exception-routing |
| [UC-OP-05 — Dispatch maintenance requests using described symptoms](operations-and-support.md#uc-op-05) | P | Choice; Noul; dispatch; maintenance |
| [UC-OP-06 — Route return requests to the right evidence workflow](operations-and-support.md#uc-op-06) | P | Choice; Noul; returns; evidence-routing |
| [UC-CC-01 — Tag product attributes from explicit descriptions](commerce-and-content.md#uc-cc-01) | P | Noul; Choice; multi-label; product-attributes |
| [UC-CC-02 — Match products against explicit user constraints](commerce-and-content.md#uc-cc-02) | P | Noul; Score; constrained-matching; preferences |
| [UC-CC-03 — Detect inconsistency between listing text and structured fields](commerce-and-content.md#uc-cc-03) | P | Choice; consistency-check; catalog-quality |
| [UC-CC-04 — Assist content-moderation review under an explicit policy](commerce-and-content.md#uc-cc-04) | P | Noul; Choice; Score; moderation; human-review |
| [UC-CC-05 — Score drafts against an editorial brief](commerce-and-content.md#uc-cc-05) | P | Score; Noul; editorial-review; reusable-signals |
| [UC-CC-06 — Code customer feedback by topic and expressed stance](commerce-and-content.md#uc-cc-06) | P | Noul; Choice; feedback; topic-sentiment |
| [UC-SW-01 — Prioritize code changes for deeper review](software-and-security.md#uc-sw-01) | P | Noul; Score; Choice; review-triage |
| [UC-SW-02 — Navigate a repository toward relevant files](software-and-security.md#uc-sw-02) | P | Score; Choice; code-search; hierarchical-navigation |
| [UC-SW-03 — Classify operational alerts and suggest incident linkage](software-and-security.md#uc-sw-03) | P | Choice; alert-routing; incident-triage |
| [UC-SW-04 — Add a semantic preflight check before an agent action](software-and-security.md#uc-sw-04) | P | Choice; Noul; guardrails; authorization-boundary |
| [UC-SW-05 — Check semantic acceptance criteria in software tests](software-and-security.md#uc-sw-05) | P | Noul; Choice; semantic-testing; verification |
| [UC-SW-06 — Use semantic predicates in a CLI or data pipeline](software-and-security.md#uc-sw-06) | E | Noul; Choice; CLI; batch-processing |
| [UC-RA-01 — Screen titles and abstracts for a research review](research-and-analytics.md#uc-ra-01) | P | Noul; Choice; research-screening; evidence |
| [UC-RA-02 — Classify the role of an evidence passage](research-and-analytics.md#uc-ra-02) | P | Choice; Noul; evidence-classification; research |
| [UC-RA-03 — Apply a qualitative codebook to open-ended responses](research-and-analytics.md#uc-ra-03) | P | Noul; Choice; qualitative-coding; surveys |
| [UC-RA-04 — Turn text into features for a separate predictive model](research-and-analytics.md#uc-ra-04) | W | Noul; Score; semantic-features; supervised-ML |
| [UC-RA-05 — Select informative cases for human labeling and threshold review](research-and-analytics.md#uc-ra-05) | P | uncertainty; active-learning; evaluation; human-review |
| [UC-RA-06 — Monitor semantic drift and audit incoming datasets](research-and-analytics.md#uc-ra-06) | P | Choice; Noul; Score; monitoring; dataset-quality |
| [UC-IN-01 — Choose the next bounded mobile-interface action](interactive-systems.md#uc-in-01) | E | Choice; Noul; mobile-agent; state-loop |
| [UC-IN-02 — Interpret smart-home commands over a known device inventory](interactive-systems.md#uc-in-02) | W | Choice; Noul; fan-out; device-control |
| [UC-IN-03 — Compose an interface from preapproved components](interactive-systems.md#uc-in-03) | E | Choice; Noul; UI-composition; bounded-generation |
| [UC-IN-04 — Choose among legally available game moves](interactive-systems.md#uc-in-04) | E | Choice; games; bounded-actions; state-loop |
| [UC-IN-05 — Select non-player-character behavior from designer-defined actions](interactive-systems.md#uc-in-05) | P | Choice; Score; NPC; interactive-behavior |
| [UC-IN-06 — Choose the next useful clarification in a form](interactive-systems.md#uc-in-06) | P | Choice; Noul; adaptive-forms; clarification |
| [UC-SP-01 — Prepare contract-clause deviations for legal review](specialist-review.md#uc-sp-01) | P | Choice; Noul; legal-review-support; evidence-packets |
| [UC-SP-02 — Organize an insurance claim’s evidence packet](specialist-review.md#uc-sp-02) | P | Choice; insurance-review-support; evidence-completeness |
| [UC-SP-03 — Locate explicit skill evidence for a recruiting reviewer](specialist-review.md#uc-sp-03) | P | Choice; recruiting-review-support; evidence-extraction |
| [UC-SP-04 — Check administrative completeness of a clinical document packet](specialist-review.md#uc-sp-04) | P | Choice; administrative-healthcare; completeness-check |
| [UC-SP-05 — Route financial anomaly narratives to a review category](specialist-review.md#uc-sp-05) | P | Choice; Noul; financial-review-support; triage |
| [UC-SP-06 — Code technician observations for quality-review workflows](specialist-review.md#uc-sp-06) | P | Noul; Choice; quality-review; observation-coding |

## Turn a card into an implementation

Read one relevant card and its primary sources, then fill the [decision specification](../templates/decision-spec-template.md). Replace the fictional input and policy with actual requirements. Resolve missing candidate coverage, uncertainty and authorization before choosing thresholds. Read the current [integration contract](../integration-map.md) immediately before coding.

The catalog is not exhaustive and should not constrain brainstorming to named industries. Transfer the underlying pattern to another domain only after identifying different evidence requirements and consequences. For new cards, use the [use-case template](../templates/use-case-template.md).
