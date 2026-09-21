# Agents and routing

[Handbook index](../index.md) · [All use cases](index.md) · [Evidence labels](index.md#evidence-labels)

Handler selection, agent harnesses, escalation and context management.

Research reviewed: 2026-09-21. All concrete inputs are hypothetical; no numeric model outputs or performance results are asserted. Question lists specify meaning, not a copy-paste API payload. Suggested operating controls are design adaptations, not claims about a linked project’s exact implementation. Apply the shared [decision semantics](../decision-design.md) and [evaluation controls](../evaluation-and-operations.md).

## On this page


- [UC-AG-01 — Route a request to an appropriate handler](#uc-ag-01) · **W**

- [UC-AG-02 — Choose a known tool and bounded arguments](#uc-ag-02) · **W**

- [UC-AG-03 — Discover a relevant agent skill without loading the whole library](#uc-ag-03) · **P**

- [UC-AG-04 — Route work between a cheaper model, a stronger model and a human](#uc-ag-04) · **E**

- [UC-AG-05 — Verify inexpensive extraction before escalating](#uc-ag-05) · **W**

- [UC-AG-06 — Prune tool history while preserving retained text](#uc-ag-06) · **E**


---

<a id="uc-ag-01"></a>

## UC-AG-01 — Route a request to an appropriate handler

**Tags:** Choice; Noul; fan-out; handler-routing

**Evidence basis — W:** Official worked pattern; the concrete scenario and operating policy here are illustrative adaptations, not reproduced benchmark results.

### Problem and Jev’s role

An assistant has several existing capabilities. The semantic task is to select a handler, not to generate the final answer or redesign the application.

### Concrete input example

`request`: “Please send me the receipt for order O-42, and explain the warranty.” `handlers`: order-document lookup, knowledge search, account update and human support, each with descriptions. Include authenticated account scope and the conversation turn that establishes what “it” refers to.

### Questions to ask

- Noul: “Does `request` contain more than one independently actionable request?”
- Choice: “For a single request, which listed handler can address it?” Options describe each capability plus `NEEDS_CLARIFICATION` and `OUT_OF_SCOPE`. This question is speculative when the input is compound.

### Interpretation and system behavior

A compound result sends the request to a constrained splitter or a human. Route each resulting atomic request independently. For this example, an order lookup supplies the receipt and a retrieval pipeline supplies warranty evidence. Code checks order ownership before retrieving anything; a separate component writes any explanation.

### Boundaries and failure modes

A confident handler selection does not prove the handler has the necessary record or permission. Do not silently discard a second intent. Avoid a model call when an explicit UI action already identifies the handler.

### How to evaluate

Measure per-handler confusion and complete coverage of compound requests. Include ambiguous pronouns, unsupported requests and a correct intent combined with an unauthorized object.

### Sources and related cases

[Intent routing](https://docs.typesafe.ai/patterns/intent-routing) · [Speculative fan-out](https://docs.typesafe.ai/patterns/fan-out)

Related: [UC-AG-02](agents-and-routing.md#uc-ag-02) · [UC-OP-01](operations-and-support.md#uc-op-01).


---

<a id="uc-ag-02"></a>

## UC-AG-02 — Choose a known tool and bounded arguments

**Tags:** Choice; tool-selection; bounded-arguments

**Evidence basis — W:** Official worked pattern; the concrete scenario and operating policy here are illustrative adaptations, not reproduced benchmark results.

### Problem and Jev’s role

A natural-language interface needs to invoke existing typed functions without asking a language model to invent arbitrary tool-call JSON.

### Concrete input example

`request`: “Show failed uploads from yesterday as a table.” Supply allowed read-only tools, allowed status filters, available display modes, the reference date and timezone. A parser or UI provides candidate date expressions; the clock remains authoritative.

### Questions to ask

- Choice: “Which allowed read-only operation matches `request`?” Options: describe search-uploads, summarize-uploads, show-upload-details, and no-supported-operation.
- Choice: “Assuming this is an upload search, which status is requested?” Options: failed, succeeded, all, unspecified.
- Choice: “Which available presentation does the user request?” Options: table, chart, unspecified.

### Interpretation and system behavior

Code consumes only the arguments for the selected operation, resolves “yesterday” with calendar logic, validates the complete function schema and checks read permissions. It then executes the ordinary search function. Missing required arguments produce a clarification, not a guessed string.

### Boundaries and failure modes

This pattern does not make Jev a general free-text argument generator. Arbitrary search phrases need verbatim spans, a separate generative component or user input. Independent speculative answers can be inconsistent; validate the assembled call as a whole.

### How to evaluate

Test unsupported operations, multiple filters, timezone boundaries, conflicting arguments and safe rejection of values absent from the allowed domain. Verify the actual function called, not only the selected label.

### Sources and related cases

[Bounded function calling](https://docs.typesafe.ai/cookbooks/function_calling) · [Structured instructions and criteria](https://docs.typesafe.ai/primitives/advanced)

Related: [UC-DD-02](data-and-documents.md#uc-dd-02) · [UC-SW-04](software-and-security.md#uc-sw-04).


---

<a id="uc-ag-03"></a>

## UC-AG-03 — Discover a relevant agent skill without loading the whole library

**Tags:** Score; Choice; Noul; skill-selection; retrieval

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

An agent has a large skill catalog and should load only potentially useful instructions. The linked official cookbook is described in the index, but its full body was unavailable during this research.

### Concrete input example

`task`: “Produce an accessible HTML email from this approved content.” `skill_catalog`: stable IDs, concise capability descriptions, supported environments and trust metadata. A second stage receives the full text of shortlisted skills, not just their names.

### Questions to ask

- Score per catalog item: “How directly does this skill support the stated deliverable?” Levels: unrelated; partially helpful; directly covers the requested capability.
- Noul: “Does this task require a specialist skill beyond the agent’s already available instructions?”
- Second-stage Choice: “Which inspected skill is applicable in the current environment?” Include no-suitable-skill.

### Interpretation and system behavior

Shortlist by semantic relevance, fetch only the shortlisted definitions, then reassess using the full capability and environment details. Code resolves stable IDs and enforces an approved-source policy before loading a skill. The appropriate outcome may be to load nothing.

### Boundaries and failure modes

This is an original design proposal inspired by an indexed example, not a reproduced implementation. Descriptive relevance is not evidence that a skill is trustworthy. A skill that cannot run on the host should not be selected merely because its title matches.

### How to evaluate

Test selection accuracy, unnecessary skill loads and missed necessary skills. Include deceptive descriptions, overlapping skills, stale catalog entries and environment incompatibility.

### Sources and related cases

[Skill suggestion](https://docs.typesafe.ai/cookbooks/skill_suggestion) · [Live documentation index](https://docs.typesafe.ai/llms.txt) · [Score](https://docs.typesafe.ai/primitives/score)

Related: [UC-SR-01](search-and-knowledge.md#uc-sr-01) · [UC-AG-02](agents-and-routing.md#uc-ag-02).


---

<a id="uc-ag-04"></a>

## UC-AG-04 — Route work between a cheaper model, a stronger model and a human

**Tags:** Score; Choice; Noul; model-routing; cascade

**Evidence basis — E:** External implementation or experiment inspected through its primary documentation; not executed, independently reproduced or security-audited here.

### Problem and Jev’s role

A generative application needs a routing layer. LangChain documents a TypeSafe classifier integration and Jev-oriented harness components; the design below is an illustrative routing policy, not a measured optimum.

### Concrete input example

`task`: “Explain the failing integration test using this log and two relevant source files.” Include task scope, available evidence, required output, tool availability, consequence of a wrong answer and each handler’s documented capability.

### Questions to ask

- Score: “How much cross-source reasoning does this task require?” Levels: direct transformation of supplied content; local comparison across supplied items; substantial multi-step investigation.
- Noul: “Is essential evidence missing from the supplied material?”
- Choice: “Which available handler is suitable under the supplied routing policy?” Include evidence-collection and human-review routes.

### Interpretation and system behavior

Route straightforward, well-supported work to the smaller generative handler. Send missing-evidence cases to retrieval rather than assuming a more expensive model knows the answer. Reserve a human route for policy-defined consequences. Log the downstream success to evaluate the router itself.

### Boundaries and failure modes

An apparent difficulty estimate is not a proven probability of another model succeeding. A routing call adds overhead and can be wasteful for uniformly simple traffic. Keep model names, prices and provider availability in deployment configuration.

### How to evaluate

Compare end-to-end quality and total cost against always-small and always-strong baselines. Measure false downgrades, unnecessary escalations and outcomes on different task families.

### Sources and related cases

[LangChain TypeSafe integration](https://docs.langchain.com/oss/python/integrations/providers/typesafe) · [LangChain: Building a Harness with Jev](https://www.langchain.com/blog/building-a-harness-with-jev) · [Confidence-gated routing](https://docs.typesafe.ai/patterns/confidence-routing)

Related: [UC-AG-05](agents-and-routing.md#uc-ag-05) · [UC-RA-05](research-and-analytics.md#uc-ra-05).


---

<a id="uc-ag-05"></a>

## UC-AG-05 — Verify inexpensive extraction before escalating

**Tags:** Noul; Choice; verification; cascade

**Evidence basis — W:** Official worked pattern; the concrete scenario and operating policy here are illustrative adaptations, not reproduced benchmark results.

### Problem and Jev’s role

A separate generative model extracts structured fields from messy text. Jev can provide field-specific checks before accepting the result or asking a more capable extractor to retry.

### Concrete input example

`source`: a supplier message containing an original order reference and a replacement order reference. `candidate_record`: the small extractor’s proposed active order reference and promised shipment date. Preserve offsets and the exact original text.

### Questions to ask

- Noul per field: “Does `candidate_record.active_order_reference` refer to the active replacement order in `source`, rather than a historical order?”
- Choice per required field: “What does the supplied evidence establish?” Options: supports the candidate; contradicts the candidate; does not establish a value.

### Interpretation and system behavior

Code first validates types and exact-span provenance. Jev then checks semantic roles. Supported fields can proceed under the evaluated policy; uncertain or contradicted fields trigger a retry with the source and a machine-readable error category. A second extractor’s answer is checked again rather than automatically accepted.

### Boundaries and failure modes

Generator and verifier can share failure modes. A verifier cannot detect an omitted document it never sees. Do not substitute model approval for exact identifiers, amount arithmetic or final authorization to write a record.

### How to evaluate

Evaluate final field accuracy, omission recall, wrong-field acceptance, escalation workload and total pipeline cost. Include plausible but unsupported values and cases where both models agree on the same wrong role.

### Sources and related cases

[Structured-data-extraction cascade](https://docs.typesafe.ai/cookbooks/sde_cascade)

Related: [UC-DD-01](data-and-documents.md#uc-dd-01) · [UC-SR-04](search-and-knowledge.md#uc-sr-04).


---

<a id="uc-ag-06"></a>

## UC-AG-06 — Prune tool history while preserving retained text

**Tags:** Noul; memory; context-pruning; provenance

**Evidence basis — E:** External implementation or experiment inspected through its primary documentation; not executed, independently reproduced or security-audited here.

### Problem and Jev’s role

A long-running agent accumulates obsolete tool calls. The inspected fast-jev-compaction repository uses Jev judgments to keep, truncate or remove tool-call/result pairs rather than rewrite retained text.

### Concrete input example

Illustrative input: an old directory listing, a failed command, the latest passing test and the current task. Supply paired call IDs, dependencies and explicit protected records. The inspected project’s decision state abbreviates result bodies; that information loss must be considered.

### Questions to ask

- Noul per eligible call: “Is knowing that this call occurred still necessary for the current task?”
- Noul per eligible result: “Does the exact result contain information still needed for the current task?” If the body was omitted, avoid treating metadata alone as sufficient evidence of irrelevance.

### Interpretation and system behavior

For a deployment of this pattern, the recommended adaptation is to pin instructions, irreversible-action receipts and non-recoverable evidence before asking Jev. It preserves call/result pairing, archives the full transcript and changes only the agent’s working view. When deletion cannot be justified, keep the item or retrieve its body for a second assessment.

### Boundaries and failure modes

The repository was inspected, not executed. Its animated showcase is scripted. Retaining text verbatim avoids rewriting errors but does not eliminate harmful omissions. Re-running a historical tool may be impossible or return a changed result.

### How to evaluate

Replay tasks with and without pruning. Measure task completion, lost constraints, invalid call/result sequences and recovery of deleted evidence, alongside context reduction.

### Sources and related cases

[fast-jev-compaction](https://github.com/tamaratran/fast-jev-compaction)

Related: [UC-SR-05](search-and-knowledge.md#uc-sr-05) · [UC-SW-02](software-and-security.md#uc-sw-02).


[Back to all use cases](index.md) · [Handbook index](../index.md)
