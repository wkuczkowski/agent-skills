# Jev agent handbook

**Purpose:** help an AI agent discover useful Jev applications, design bounded decisions and find the current implementation source without loading a copied SDK manual.

**Audience:** AI agents. **Language:** English. **Companion:** the agent is assumed to have the [official TypeSafe skill](https://raw.githubusercontent.com/typesafe-ai/skills/main/skills/typesafe-ai/SKILL.md). This package supplements that skill; it does not replace it or bundle a frozen duplicate.

**Contents:** 54 developed use cases in 9 areas, architecture guidance, evaluation and operational controls, a source map and reusable design templates. **Research reviewed:** 2026-09-21. Detailed [research boundaries](sources.md) are explicit.

## Reading contract for the consuming agent

Use this index to select relevant material, not as a requirement to load the whole package. For a specific request, read one or two matching cards, the relevant design section and the current upstream contract. For open-ended discovery, scan the [use-case index](use-cases/index.md) before expanding selected areas.

The local pages explain durable design choices and original examples. The [live TypeSafe documentation index](https://docs.typesafe.ai/llms.txt) is the discovery root for changing behavior, models, APIs and SDKs. The current contract for the chosen integration, not a historical cookbook or this handbook’s example wording, governs implementation.

Keep known rules, exact data operations and side-effect authorization in code. Choose an uncertainty and review policy based on the actual consequences. Do not treat a use-case card as evidence of production readiness or import its hypothetical policy without evaluation.

## Task-oriented routes

| Your current task | Read locally | Then verify upstream |
|---|---|---|
| Explore possible applications | [Use-case index](use-cases/index.md), selected domain files. | The matching original sources and current official cookbooks. |
| Decide whether Jev fits | [Decision design](decision-design.md), [limitations](limitations-and-non-goals.md). | Current primitives, state and model capabilities. |
| Design a workflow | [Architecture patterns](architecture-patterns.md), [decision template](templates/decision-spec-template.md). | Relevant pattern and closest cookbook. |
| Implement in an existing stack | [Integration map](integration-map.md), selected case. | Current SDK or wrapper reference and installed types. |
| Debug unexpected results | [Failure diagnosis](limitations-and-non-goals.md), [evaluation](evaluation-and-operations.md). | Exact request contract and current version-specific limitations. |
| Deploy or change a model | [Evaluation and operations](evaluation-and-operations.md), [maintenance](maintenance.md). | Current provider route, models, agreements and release notes. |
| Add another use case | [Use-case template](templates/use-case-template.md), [source registry](sources.md). | Primary evidence for the claimed pattern or implementation. |

## Package map

| File or directory | What it contains |
|---|---|
| [index.md](index.md) | This entry point and targeted reading routes. |
| [decision-design.md](decision-design.md) | Fit, question semantics, state, candidates and decision policy. |
| [architecture-patterns.md](architecture-patterns.md) | Fan-out, selection, verification, hierarchies, reusable scores and control loops. |
| [integration-map.md](integration-map.md) | Native SDK/API routes, frameworks, gateways, MCP and CLI references. |
| [evaluation-and-operations.md](evaluation-and-operations.md) | Dataset design, useful metrics, thresholds, rollout, telemetry and recovery. |
| [limitations-and-non-goals.md](limitations-and-non-goals.md) | Capability checks, common incorrect inferences and failure diagnosis. |
| [sources.md](sources.md) | 67 source URLs, source status and coverage of all 18 cookbooks in the inspected index. |
| [maintenance.md](maintenance.md) | What must be refreshed and how to avoid freezing volatile details. |
| [use-cases/index.md](use-cases/index.md) | Full case directory, evidence labels and cross-cutting entry points. |
| `use-cases/` domain files | Detailed cards with inputs, questions, interpretation, boundaries, evaluation and links. |
| [templates/decision-spec-template.md](templates/decision-spec-template.md) | A task-to-implementation design record. |
| [templates/use-case-template.md](templates/use-case-template.md) | A consistent format for extending this knowledge base. |

## Domain files

- [Agents and routing](use-cases/agents-and-routing.md) — Handler selection, agent harnesses, escalation and context management.
- [Search and knowledge](use-cases/search-and-knowledge.md) — Retrieval, evidence selection, citations and reuse of existing answers.
- [Data and documents](use-cases/data-and-documents.md) — Extractive transformations, entity linking and taxonomies.
- [Operations and support](use-cases/operations-and-support.md) — Work queues, tickets, commitments and exception handling.
- [Commerce and content](use-cases/commerce-and-content.md) — Catalogs, matching, editorial review and feedback.
- [Software and security](use-cases/software-and-security.md) — Developer workflows, observability and bounded security checks.
- [Research and analytics](use-cases/research-and-analytics.md) — Screening, coding, semantic features and evaluation data.
- [Interactive systems](use-cases/interactive-systems.md) — Mobile actions, home controls, constrained UI, games and forms.
- [Specialist review workflows](use-cases/specialist-review.md) — Evidence preparation for qualified reviewers, not autonomous consequential decisions.

## Evidence and freshness

Use-case labels distinguish an official worked pattern (**W**), an inspected external implementation or experiment (**E**) and an original proposed application (**P**). They indicate provenance, not measured effectiveness. All concrete input examples are hypothetical. No live API benchmarks, project installations or security audits were performed for this package.

Question lists are conceptual specifications, not versioned JSON payloads. Prices, model identifiers, limits, package versions, default thresholds and hosting promises are intentionally left in live sources. The package also records unread indexed pages and fetch failures instead of implying that every source was fully accessible.

Begin with [the use-case index](use-cases/index.md) for discovery or [the integration map](integration-map.md) for a concrete implementation task.
