# Software and security

[Handbook index](../index.md) · [All use cases](index.md) · [Evidence labels](index.md#evidence-labels)

Developer workflows, observability and bounded security checks.

Research reviewed: 2026-09-21. All concrete inputs are hypothetical; no numeric model outputs or performance results are asserted. Question lists specify meaning, not a copy-paste API payload. Suggested operating controls are design adaptations, not claims about a linked project’s exact implementation. Apply the shared [decision semantics](../decision-design.md) and [evaluation controls](../evaluation-and-operations.md).

## On this page


- [UC-SW-01 — Prioritize code changes for deeper review](#uc-sw-01) · **P**

- [UC-SW-02 — Navigate a repository toward relevant files](#uc-sw-02) · **P**

- [UC-SW-03 — Classify operational alerts and suggest incident linkage](#uc-sw-03) · **P**

- [UC-SW-04 — Add a semantic preflight check before an agent action](#uc-sw-04) · **P**

- [UC-SW-05 — Check semantic acceptance criteria in software tests](#uc-sw-05) · **P**

- [UC-SW-06 — Use semantic predicates in a CLI or data pipeline](#uc-sw-06) · **E**


---

<a id="uc-sw-01"></a>

## UC-SW-01 — Prioritize code changes for deeper review

**Tags:** Noul; Score; Choice; review-triage

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A development workflow needs to focus reviewer attention, while compilers, tests, static analysis and humans remain responsible for determining correctness.

### Concrete input example

`diff`: a change to retry handling that now retries an operation after a timeout. Include the relevant function context, operation semantics, test changes and the review policy. Avoid submitting the entire repository when the affected behavior is identifiable.

### Questions to ask

- Noul: “Does this change affect when an externally visible operation may be repeated?”
- Score: “How much evidence of changed failure-path behavior is present?” Levels: no relevant behavior change; localized failure-path change; cross-component or stateful failure-path change.
- Choice: “Which specialist review area is relevant?” Options describe reliability, security, data integrity, routine maintenance and unresolved.

### Interpretation and system behavior

Code attaches review labels and requests attention to specific changed regions. A generative reviewer or engineer investigates the retry semantics. Existing failing tests or security checks remain blocking regardless of a low Jev risk signal.

### Boundaries and failure modes

This is a proposed triage aid, not a vulnerability detector with demonstrated recall. A small diff can have major effects outside the supplied context. Do not auto-merge based on a low score or present an unverified model suspicion as a confirmed defect.

### How to evaluate

Use historical changes with independently adjudicated outcomes. Measure missed important changes, reviewer workload and whether the selected review area helps identify actual issues.

### Sources and related cases

[Composite scoring](https://docs.typesafe.ai/patterns/composite-scoring) · [Confidence-gated routing](https://docs.typesafe.ai/patterns/confidence-routing)

Related: [UC-SW-03](software-and-security.md#uc-sw-03) · [UC-SW-04](software-and-security.md#uc-sw-04).


---

<a id="uc-sw-02"></a>

## UC-SW-02 — Navigate a repository toward relevant files

**Tags:** Score; Choice; code-search; hierarchical-navigation

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A coding agent wants candidate files without immediately loading a large repository into its context.

### Concrete input example

`task`: “Locate the validation that rejects expired invitation links.” Supply a repository snapshot ID, directory summaries, file paths and selected symbol metadata. Exact filename and symbol search should run first when the task names them.

### Questions to ask

- Score per candidate path: “How directly does the available evidence connect this file or directory to invitation-expiry validation?” Levels describe unrelated, plausible supporting code and directly relevant implementation.
- Choice after reading shortlisted files: “Which available source region should be inspected next?” Include insufficient-evidence and no-relevant-region.

### Interpretation and system behavior

Code explores a bounded frontier, reads candidate files and reassesses using actual content. It keeps a fallback to ordinary text search and records which evidence led to each branch. The output is navigation guidance, not a code modification.

### Boundaries and failure modes

Names and summaries can be misleading. Narrow greedy traversal can miss cross-cutting middleware or configuration. The same codebase snapshot must be used when resolving paths and source locations.

### How to evaluate

Test unfamiliar repository layouts, misleading names, generated files, cross-module logic and empty search results. Measure relevant-file recall and total tool reads, not just first-choice accuracy.

### Sources and related cases

[Hierarchical classification](https://docs.typesafe.ai/cookbooks/hierarchical_classification) · [Line-by-line semantic search](https://docs.typesafe.ai/cookbooks/semantic_find)

Related: [UC-SR-02](search-and-knowledge.md#uc-sr-02) · [UC-AG-06](agents-and-routing.md#uc-ag-06).


---

<a id="uc-sw-03"></a>

## UC-SW-03 — Classify operational alerts and suggest incident linkage

**Tags:** Choice; alert-routing; incident-triage

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

An on-call system receives noisy text alerts and needs a better triage queue. Jev can interpret summaries while exact alert thresholds and event-time calculations remain deterministic.

### Concrete input example

`alert`: connection-pool exhaustion following a deployment. Supply sanitized stack excerpts, service ownership, metric observations, timestamps and candidate incidents. Separate measured facts from an automated narrative that may already contain a guess.

### Questions to ask

- Choice: “Which documented operational category matches the observed symptoms?” Options: capacity, dependency failure, deployment regression, data issue, unknown.
- Choice per candidate incident: “Are these observations consistent with the same incident, a related symptom or a separate event?” Include insufficient evidence.

### Interpretation and system behavior

Code routes to the responsible team and suggests links, with the raw alert preserved. It keeps pre-existing deterministic paging conditions and does not suppress an alert merely because a model groups it with a known issue. Engineers establish root cause.

### Boundaries and failure modes

Symptom classification is not causal diagnosis. Clock skew, incomplete telemetry and copied stack fragments can cause false grouping. Sensitive tokens and customer data should be removed before sending logs.

### How to evaluate

Replay incidents with independent labels. Measure missed pages, wrong incident links, unnecessary pages and time to useful context. Include concurrent unrelated failures after the same deployment.

### Sources and related cases

[Intent routing](https://docs.typesafe.ai/patterns/intent-routing) · [Knowledge-graph entity alignment](https://docs.typesafe.ai/cookbooks/entity_alignment)

Related: [UC-OP-02](operations-and-support.md#uc-op-02) · [UC-SW-01](software-and-security.md#uc-sw-01).


---

<a id="uc-sw-04"></a>

## UC-SW-04 — Add a semantic preflight check before an agent action

**Tags:** Choice; Noul; guardrails; authorization-boundary

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

An agent proposes a tool action after reading untrusted content. A separate semantic check can flag mismatch with the user’s task, but cannot replace permissions or input validation.

### Concrete input example

`authorized_goal`: export a summary for internal review. `proposed_action`: send the raw dataset to an external recipient. Supply the relevant user-approved scope and tool arguments separately from retrieved text. Do not let an untrusted document rewrite the approved goal.

### Questions to ask

- Choice: “How does this proposed action relate to the authorized goal?” Options: within the stated scope; exceeds the scope; unrelated; insufficient authorization detail.
- Noul: “Would this action disclose information to a destination not established by the approved task?”

### Interpretation and system behavior

Code first checks identity, resource scope, destination allowlists and schema validity. Jev adds an advisory semantic signal. A mismatch or ambiguity pauses the action for confirmation; a favorable result cannot override a failed deterministic control. Log the action proposal and final authorization outcome.

### Boundaries and failure modes

The model and the guard can both be manipulated. Typed output restricts format, not trust. This is not an injection-proof architecture or a security audit. Review destructive or sensitive actions according to independent policy.

### How to evaluate

Test scope drift, malicious instructions inside documents, benign quoted instructions and ambiguous user approval. Measure actual unauthorized-action prevention across the whole system, not detector accuracy alone.

### Sources and related cases

[Guardrails for LLMs](https://docs.typesafe.ai/cookbooks/llm_guardrails) · [OWASP: LLM prompt-injection prevention](https://cheatsheetseries.owasp.org/cheatsheets/LLM_Prompt_Injection_Prevention_Cheat_Sheet.html) · [Version-specific Jev jaggedness report](https://docs.typesafe.ai/model-jaggedness/jev-1.13)

Related: [UC-AG-02](agents-and-routing.md#uc-ag-02) · [UC-SR-03](search-and-knowledge.md#uc-sr-03).


---

<a id="uc-sw-05"></a>

## UC-SW-05 — Check semantic acceptance criteria in software tests

**Tags:** Noul; Choice; semantic-testing; verification

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

Some acceptance criteria concern meaning rather than an exact string. A semantic assertion can supplement, not replace, deterministic tests.

### Concrete input example

`requirement`: “The failed-payment screen must tell the user that no charge was made and provide a recovery action.” `observed_ui`: text and accessible controls captured from a particular test run. Screenshots require a separate extraction or perception component.

### Questions to ask

- Noul: “Does the observed message clearly state that no charge was made?”
- Noul: “Does the captured interface offer an identifiable action to recover from this failure?”
- Choice: “Is the observation complete enough to evaluate these criteria?” Options: complete; missing relevant region; ambiguous capture.

### Interpretation and system behavior

Keep exact status-code, balance, DOM and accessibility assertions in ordinary tests. Route semantic uncertainty to an explicit inconclusive result or human review, rather than silently passing. Store the UI snapshot and question version with failures for reproduction.

### Boundaries and failure modes

A semantic assertion is probabilistic and may be flaky. Repeating until a pass appears hides failures. It cannot establish that no charge occurred in the backend merely because the message says so; that requires a separate state assertion.

### How to evaluate

Test paraphrases, contradictory messages, hidden recovery controls and incomplete captures. Measure false passes, false failures and repeatability on fixed snapshots.

### Sources and related cases

[Noul](https://docs.typesafe.ai/primitives/noul) · [Confidence](https://docs.typesafe.ai/confidence)

Related: [UC-SW-01](software-and-security.md#uc-sw-01) · [UC-RA-05](research-and-analytics.md#uc-ra-05).


---

<a id="uc-sw-06"></a>

## UC-SW-06 — Use semantic predicates in a CLI or data pipeline

**Tags:** Noul; Choice; CLI; batch-processing

**Evidence basis — E:** External implementation or experiment inspected through its primary documentation; not executed, independently reproduced or security-audited here.

### Problem and Jev’s role

The SemDecide project exposes Jev-backed decisions for text and JSONL pipelines. A useful design keeps semantic selection separate from shell execution.

### Concrete input example

Illustrative input: sanitized release-note records from standard input. The desired predicate is whether a record announces an action users must take before upgrading. Preserve record IDs and capture tool errors separately from classification results.

### Questions to ask

- Noul per record: “Does this release note explicitly require an action before upgrading?”
- Choice: “What kind of action is stated?” Options: configuration change; data migration; credential change; no required action; unclear.

### Interpretation and system behavior

A read-only pipeline writes matching records and a review stream for uncertain cases. The wrapper translates responses into its documented output and exit-code contract. A later script may act only on independently validated data; model-selected text is never interpolated into an executable command.

### Boundaries and failure modes

The community CLI was not run or audited here. Installation, flags and exit codes must be read from its current documentation. A provider timeout is a service failure, not a semantic “false” and not an empty successful dataset.

### How to evaluate

Test pipeline behavior on empty input, malformed JSONL, uncertain results, provider failure and adversarial text containing shell syntax. Evaluate missed required actions and correct preservation of record IDs.

### Sources and related cases

[SemDecide](https://github.com/sharziki/semdecide) · [Noul](https://docs.typesafe.ai/primitives/noul)

Related: [UC-AG-01](agents-and-routing.md#uc-ag-01) · [UC-RA-06](research-and-analytics.md#uc-ra-06).


[Back to all use cases](index.md) · [Handbook index](../index.md)
