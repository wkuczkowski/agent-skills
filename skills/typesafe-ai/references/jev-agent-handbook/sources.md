# Source registry and research boundaries

[Handbook index](index.md) · [Use-case index](use-cases/index.md) · [Maintenance](maintenance.md)

Research reviewed: **2026-09-21**. The registry contains **67 unique source URLs**. It is a curated navigation map, not a claim that every linked subpage, repository file or legal agreement was exhaustively inspected.

## How to interpret source status

**Read** means the page’s relevant body or substantive documentation sections were inspected. It does not mean code was executed or audited. **Indexed** means the reference was found in the official documentation index but its full body was not individually reviewed. **Body unavailable** records a failed fetch; the page is retained for rediscovery and its unread details are not asserted.

The handbook’s detailed workflows, policy suggestions, fictional inputs and test cases are original design material. A linked source may support the underlying pattern without implementing that exact scenario. No API benchmarks were reproduced, no deployment outcomes measured and no linked repository certified safe.

## Core TypeSafe references

| Source | Status | Scope and use |
|---|---|---|
| [Official TypeSafe agent skill](https://raw.githubusercontent.com/typesafe-ai/skills/main/skills/typesafe-ai/SKILL.md) | Read | Companion dependency; linked, not copied into this package. |
| [Introduction](https://docs.typesafe.ai/introduction) | Read | Product orientation; not a capability benchmark. |
| [Live documentation index](https://docs.typesafe.ai/llms.txt) | Read | Discovery entry point; refresh before implementing. |
| [State design](https://docs.typesafe.ai/concepts/state) | Read | Input representation and evidence boundaries. |
| [Question primitives](https://docs.typesafe.ai/primitives) | Read | Choose a question by its meaning, not its desired JSON shape. |
| [Choice](https://docs.typesafe.ai/primitives/choice) | Read | Closed-set selection and its distribution. |
| [Score](https://docs.typesafe.ai/primitives/score) | Read | Ordered descriptive levels, expected position and distribution. |
| [Noul](https://docs.typesafe.ai/primitives/noul) | Read | Probability of yes; distinct from a severity scale. |
| [Confidence](https://docs.typesafe.ai/confidence) | Read | Interpret concentration separately from correctness and authorization. |
| [Structured instructions and criteria](https://docs.typesafe.ai/primitives/advanced) | Read | Read exact accepted structures in the current contract. |
| [Official use-case map](https://docs.typesafe.ai/concepts/use-case-map) | Read | Idea map, not evidence of production deployments. |
| [Speculative fan-out](https://docs.typesafe.ai/patterns/fan-out) | Read | Shared state, independent questions, conditional consumption. |
| [Confidence-gated routing](https://docs.typesafe.ai/patterns/confidence-routing) | Read | Illustrative routing; thresholds and permissions need local policy. |
| [Composite scoring](https://docs.typesafe.ai/patterns/composite-scoring) | Read | Reusable semantic dimensions and code-owned weights. |
| [Intent routing](https://docs.typesafe.ai/patterns/intent-routing) | Read | Route between deterministic, generative and human handlers. |
| [Current models and commercial details](https://docs.typesafe.ai/models) | Read | Recheck IDs, modalities, prices and limits; this package does not freeze them. |
| [Version-specific Jev jaggedness report](https://docs.typesafe.ai/model-jaggedness/jev-1.13) | Read | Historical, model-version-specific evidence. Look for a newer report before deployment. |
| [HTTP API reference](https://docs.typesafe.ai/api) | Read | Current wire contract, validation and response semantics. |
| [Python SDK overview](https://docs.typesafe.ai/sdk/python) | Read | Current setup and source links. |
| [JavaScript SDK overview](https://docs.typesafe.ai/sdk/javascript) | Read | Current setup and source links. |
| [TypeSafe legal documents](https://docs.typesafe.ai/legal) | Read | Index reviewed, not a legal analysis of every underlying agreement. |
| [Smart-home demo](https://docs.typesafe.ai/demos/smart-home) | Read | Documentation of a demo; source availability must be rechecked. |

## Implementation pages discovered through the official index

These links provide targeted implementation entry points without duplicating every generated SDK type page. Use the live index and the SDK API navigation for the complete class, interface, exception and helper inventory.

| Source | Status | Purpose |
|---|---|---|
| [Quickstart](https://docs.typesafe.ai/introduction/quickstart) | Indexed | Follow from the current docs index. |
| [System One concepts](https://docs.typesafe.ai/concepts/system-one) | Indexed | Conceptual background. |
| [Machine-learning primer](https://docs.typesafe.ai/introduction/machine-learning-primer) | Indexed | Provider background; not a substitute for local calibration measurements. |
| [Python SDK usage](https://docs.typesafe.ai/sdk/python/usage) | Indexed | Implementation reference. |
| [Python SDK changelog](https://docs.typesafe.ai/sdk/python/changelog) | Indexed | Upgrade checkpoint. |
| [Python API reference](https://docs.typesafe.ai/sdk/python/api) | Indexed | Navigate to client, question and answer types. |
| [Python retries](https://docs.typesafe.ai/sdk/python/api/retries) | Indexed | Retry/backoff contract; do not confuse with safe action retries. |
| [Python exceptions](https://docs.typesafe.ai/sdk/python/api/exceptions) | Indexed | Transport and service error handling. |
| [JavaScript SDK changelog](https://docs.typesafe.ai/sdk/javascript/changelog) | Indexed | Upgrade checkpoint. |
| [JavaScript API reference](https://docs.typesafe.ai/sdk/javascript/api) | Indexed | Navigate to client, question and answer types. |

## Official cookbook coverage

All **18 cookbook entries** present in the inspected live index are mapped below. This is coverage of that dated index, not a promise that no newer cookbooks exist. The skill-suggestion body could not be retrieved; its index description alone was reviewed. Cookbook metrics, model names and default thresholds are deliberately not transplanted into this package.

| Official cookbook | Local design entry | Coverage note |
|---|---|---|
| [Self-consistency: Noul](https://docs.typesafe.ai/cookbooks/consistency_noul_cookbook) | [UC-RA-05](use-cases/research-and-analytics.md#uc-ra-05) | Uncertainty sampling; also evaluation and operations. |
| [Self-consistency: Choice](https://docs.typesafe.ai/cookbooks/consistency_choice_cookbook) | [UC-CC-04](use-cases/commerce-and-content.md#uc-cc-04) | Moderation review and stability; not a universal enforcement threshold. |
| [Parallel questions](https://docs.typesafe.ai/cookbooks/parallel_questions) | [UC-RA-06](use-cases/research-and-analytics.md#uc-ra-06) | Batchable record questions; architecture page explains shared-state batching. |
| [Candidate reranking](https://docs.typesafe.ai/cookbooks/rerank_typesafe) | [UC-SR-01](use-cases/search-and-knowledge.md#uc-sr-01) | Separate binary source-identity reranking from a proposed graded rubric. |
| [Line-by-line semantic search](https://docs.typesafe.ai/cookbooks/semantic_find) | [UC-SR-02](use-cases/search-and-knowledge.md#uc-sr-02) | Select source passages with an explicit no-answer path. |
| [Structure recovery](https://docs.typesafe.ai/cookbooks/autoformat) | [UC-DD-03](use-cases/data-and-documents.md#uc-dd-03) | Recover structure through decisions plus code-owned rendering. |
| [Bounded function calling](https://docs.typesafe.ai/cookbooks/function_calling) | [UC-AG-02](use-cases/agents-and-routing.md#uc-ag-02) | Select known functions and bounded arguments. |
| [Skill suggestion](https://docs.typesafe.ai/cookbooks/skill_suggestion) | [UC-AG-03](use-cases/agents-and-routing.md#uc-ag-03) | Index-described inspiration only; full body unavailable. Card is labeled P. |
| [Knowledge-graph entity alignment](https://docs.typesafe.ai/cookbooks/entity_alignment) | [UC-DD-04](use-cases/data-and-documents.md#uc-dd-04) | Candidate-pair identity assessment and reversible review. |
| [Classifying RAG passages](https://docs.typesafe.ai/cookbooks/classifying_rag_passages) | [UC-SR-03](use-cases/search-and-knowledge.md#uc-sr-03) | Passage relevance, evidence conflict and instruction-bearing content. |
| [Double-checking citations](https://docs.typesafe.ai/cookbooks/citation_check) | [UC-SR-04](use-cases/search-and-knowledge.md#uc-sr-04) | Source matching and contextual claim support. |
| [Guardrails for LLMs](https://docs.typesafe.ai/cookbooks/llm_guardrails) | [UC-SW-04](use-cases/software-and-security.md#uc-sw-04) | Related semantic screening; this action-preflight design is labeled P. |
| [Structured-data-extraction cascade](https://docs.typesafe.ai/cookbooks/sde_cascade) | [UC-AG-05](use-cases/agents-and-routing.md#uc-ag-05) | Field checks and controlled escalation. |
| [Date extraction](https://docs.typesafe.ai/cookbooks/date_extraction_cookbook) | [UC-DD-02](use-cases/data-and-documents.md#uc-dd-02) | Interpret date parts; resolve and validate with code. |
| [Pre-parsed value extraction](https://docs.typesafe.ai/cookbooks/pre_parsed_value_extraction_cookbook) | [UC-DD-01](use-cases/data-and-documents.md#uc-dd-01) | Select source candidates rather than invent a field. |
| [Hierarchical classification](https://docs.typesafe.ai/cookbooks/hierarchical_classification) | [UC-DD-05](use-cases/data-and-documents.md#uc-dd-05) | Versioned hierarchy and constrained branch exploration. |
| [Autoresearch feature discovery](https://docs.typesafe.ai/cookbooks/autoresearch_feature_discovery) | [UC-RA-04](use-cases/research-and-analytics.md#uc-ra-04) | Semantic features for a separately trained predictor. |
| [Classification using confidence](https://docs.typesafe.ai/cookbooks/classification_using_confidence) | [UC-DD-05](use-cases/data-and-documents.md#uc-dd-05) | Control reported specificity without assuming a parent is correct. |

## External projects and integrations

First-party integration documentation and original project READMEs were preferred over roundup claims. These entries establish that an integration or experiment is documented; they do not establish production readiness, reproducible performance or absence of malware.

| Source | Provenance | Scope and limitations |
|---|---|---|
| [LangChain: Building a Harness with Jev](https://www.langchain.com/blog/building-a-harness-with-jev) | Integration publisher; Read | Published 2026-09-17. Harness architecture article. |
| [LangChain TypeSafe integration](https://docs.langchain.com/oss/python/integrations/providers/typesafe) | Integration publisher; Read | Current integration documentation, not a standard chat-model interface. |
| [Vercel: TypeSafe Jev and AI SDK](https://vercel.com/kb/guide/typesafe-jev-and-ai-sdk) | Integration publisher; Read | Published 2026-09-19. Evaluation abstraction differs from the native SDK. |
| [Netlify: TypeSafe Jev in AI Gateway](https://www.netlify.com/changelog/typesafe-jev-ai-gateway/) | Integration publisher; Read | Dated announcement; verify current deployment instructions. |
| [OpenRouter TypeSafe provider listing](https://openrouter.ai/provider/typesafe) | Integration publisher; Read | Provider listing only. Not a verified integration recipe or proof of chat-completions compatibility. |
| [Droidrun mobile-jev](https://github.com/droidrun/mobile-jev) | External project; Read | README reviewed; project and its recorded demos were not executed or independently reproduced. |
| [fast-jev-compaction](https://github.com/tamaratran/fast-jev-compaction) | External project; Read | README reviewed. Its animated showcase is explicitly scripted, not a live API benchmark. |
| [itsmostafa/typesafe-mcp](https://github.com/itsmostafa/typesafe-mcp) | External project; Read | Community connector, not an official TypeSafe MCP service. README reviewed; no installation or security audit performed. |
| [Vercel Labs json-render](https://github.com/vercel-labs/json-render) | External project; Read | Repository entry and experimental Jev integration pointer reviewed. |
| [json-render: Jev integration guide](https://json-render.dev/docs/jev) | Integration publisher; Read | Experimental and unreleased at review time; check current release status instead of assuming npm availability. |
| [Maxim Saplin: first-hand Jev chess experiment](https://dev.to/maximsaplin/typesafe-jev-played-chess-and-landed-next-to-reasoning-models-28ga) | External experiment; Read | Published 2026-09-17. Different player interfaces limit cross-model interpretation. No Elo or cost claims adopted here. |
| [SemDecide](https://github.com/sharziki/semdecide) | External project; Read | README reviewed; CLI behavior is a project contract, not part of the TypeSafe API. Not executed. |

For application-level examples, start with [mobile control](use-cases/interactive-systems.md#uc-in-01), [context compaction](use-cases/agents-and-routing.md#uc-ag-06), [UI composition](use-cases/interactive-systems.md#uc-in-03), [game moves](use-cases/interactive-systems.md#uc-in-04) and [CLI decisions](use-cases/software-and-security.md#uc-sw-06). MCP is covered as an integration route rather than counted as an additional business use case.

## General methods and further discovery

| Source | Role | Boundary |
|---|---|---|
| [OWASP: LLM prompt-injection prevention](https://cheatsheetseries.owasp.org/cheatsheets/LLM_Prompt_Injection_Prevention_Cheat_Sheet.html) | General engineering reference | Security architecture reference; not an evaluation of Jev. |
| [scikit-learn: probability calibration](https://scikit-learn.org/stable/modules/calibration.html) | General engineering reference | Calibration methods and interpretation; not evidence that Jev is calibrated on a particular dataset. |
| [Community Jev project directory](https://github.com/logicrw/awesome-jev-projects) | Discovery only | Used to locate original projects. Directory claims and project counts were not independently verified. |

Community lists are useful for discovering new primary sources. Do not copy their star counts, claimed project totals, speed comparisons or security assessments as verified facts. Follow through to the original repository or author and inspect the relevant evidence.

## Access gaps and discrepancies

| Reference | What was observed | How to proceed |
|---|---|---|
| [How to build with TypeSafe](https://docs.typesafe.ai/concepts/how-to-build-with-system-one) | Body unavailable. Listed in the live index and skill; page fetch failed in this research session. No detailed claims rely on its unread body. | Rediscover from the current official index; do not rely on an unread implementation detail. |
| [Migration guide referenced by the skill](https://docs.typesafe.ai/migrating-to-v1) | Body unavailable. Fetch failed and this path was not present in the inspected index. Rediscover the current migration route; do not assume this link is usable. | Rediscover from the current official index; do not rely on an unread implementation detail. |
| [Skill suggestion](https://docs.typesafe.ai/cookbooks/skill_suggestion) | Body unavailable. Index description inspected; full cookbook body could not be fetched. | Rediscover from the current official index; do not rely on an unread implementation detail. |

Some Markdown endpoints failed while ordinary HTML pages worked. Fetch failure is not proof that a page was removed. An external article whose body could not be inspected was not used as substantive evidence. Where current integration pages and historical examples differ, the current route-specific contract and installed types take precedence; the handbook does not merge incompatible schemas.

## What this research does not establish

It does not establish Jev’s accuracy, latency, total cost, calibration, security or suitability on the reader’s dataset. It does not establish the availability of a provider account, deployment region or contractual data controls. It does not verify author-reported benchmark rankings. Use [evaluation and operations](evaluation-and-operations.md) to gather those facts for an actual implementation.
