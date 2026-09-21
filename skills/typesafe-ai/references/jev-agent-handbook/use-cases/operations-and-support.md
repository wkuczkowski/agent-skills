# Operations and support

[Handbook index](../index.md) · [All use cases](index.md) · [Evidence labels](index.md#evidence-labels)

Work queues, tickets, commitments and exception handling.

Research reviewed: 2026-09-21. All concrete inputs are hypothetical; no numeric model outputs or performance results are asserted. Question lists specify meaning, not a copy-paste API payload. Suggested operating controls are design adaptations, not claims about a linked project’s exact implementation. Apply the shared [decision semantics](../decision-design.md) and [evaluation controls](../evaluation-and-operations.md).

## On this page


- [UC-OP-01 — Classify support intent, impact and the next queue](#uc-op-01) · **P**

- [UC-OP-02 — Find duplicate tickets without erasing distinct incidents](#uc-op-02) · **P**

- [UC-OP-03 — Recognize commitments and task candidates in meeting notes](#uc-op-03) · **P**

- [UC-OP-04 — Route invoice and purchase-order exceptions](#uc-op-04) · **P**

- [UC-OP-05 — Dispatch maintenance requests using described symptoms](#uc-op-05) · **P**

- [UC-OP-06 — Route return requests to the right evidence workflow](#uc-op-06) · **P**


---

<a id="uc-op-01"></a>

## UC-OP-01 — Classify support intent, impact and the next queue

**Tags:** Choice; Score; Noul; support-triage

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A support inbox needs structured triage signals without equating emotional language with operational impact.

### Concrete input example

`ticket`: “The export fails for our entire team. We can still view records, but payroll reconciliation is blocked.” Supply the product area map, customer-provided facts, verified service telemetry and the organization’s queue definitions as separate fields.

### Questions to ask

- Choice: “Which supported product area is the reported problem about?” Include multiple-areas and unclear.
- Score: “What operational impact is established by the supplied evidence?” Levels: inconvenience with no blocked work; limited work blocked with a workaround; a material workflow blocked for the affected users.
- Noul: “Does the message explicitly request a financial remedy?”

### Interpretation and system behavior

Code combines semantic signals with verified account and incident data to select a queue. It keeps reported impact distinct from independently observed impact. A response template can acknowledge the issue, while a human or generative component handles a substantive explanation.

### Boundaries and failure modes

The Vercel guide documents a related support-evaluation pattern; this incident and policy are hypothetical. Do not infer an outage from anger alone or let a customer’s claim alter contractual priority. A refund-request flag is not permission to issue money.

### How to evaluate

Test calm reports of serious failures, angry minor issues, mixed languages and multi-topic tickets. Measure wrong-queue transfers, under-triage and downstream resolution delay.

### Sources and related cases

[Vercel: TypeSafe Jev and AI SDK](https://vercel.com/kb/guide/typesafe-jev-and-ai-sdk) · [Intent routing](https://docs.typesafe.ai/patterns/intent-routing)

Related: [UC-AG-01](agents-and-routing.md#uc-ag-01) · [UC-OP-06](operations-and-support.md#uc-op-06).


---

<a id="uc-op-02"></a>

## UC-OP-02 — Find duplicate tickets without erasing distinct incidents

**Tags:** Choice; Noul; deduplication; incident-linkage

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

Multiple reports may describe the same incident. A semantic pair check can assist deduplication after ordinary search narrows the candidate set.

### Concrete input example

`new_ticket`: a failed export after a recent deployment. `candidate_ticket`: a similar failure reported last month. Include product version, account scope, observed error identifiers and incident time range; calculate time differences in code.

### Questions to ask

- Choice: “What relationship is supported between these reports?” Options: same incident; similar symptom but distinct incident; unrelated; insufficient evidence.
- Noul: “Is there an explicit fact that rules out a shared incident?”

### Interpretation and system behavior

Suggest links rather than merge or close tickets immediately. Code prevents cross-tenant disclosure, retains individual reporters and checks incident-status compatibility. In the example, a last-month resolved incident may be useful historical context but should not absorb a fresh regression.

### Boundaries and failure modes

Similar wording is not identity. Deduplication can hide recurrence or different root causes. A missing contradiction is not positive evidence of a shared cause; preserve uncertainty and reversible links.

### How to evaluate

Use adjudicated incident pairs and measure erroneous closure risk as well as duplicate detection. Include recurring bugs, copied templates, different deployment versions and identical symptoms with different causes.

### Sources and related cases

[Knowledge-graph entity alignment](https://docs.typesafe.ai/cookbooks/entity_alignment) · [Candidate reranking](https://docs.typesafe.ai/cookbooks/rerank_typesafe)

Related: [UC-DD-04](data-and-documents.md#uc-dd-04) · [UC-SW-03](software-and-security.md#uc-sw-03).


---

<a id="uc-op-03"></a>

## UC-OP-03 — Recognize commitments and task candidates in meeting notes

**Tags:** Choice; commitment-detection; task-drafts; extraction

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

Meeting notes mix decisions, suggestions and commitments. A workflow needs task candidates with evidence, not invented assignments.

### Concrete input example

`transcript`: “Maybe we should audit the import. Priya: I will check the failed rows by Tuesday. Omar: I can help if needed.” Supply speaker identities and indexed utterances, but do not assume every named person accepted a task.

### Questions to ask

- Choice per utterance: “What kind of statement is this?” Options: explicit commitment; tentative suggestion; conditional offer; status report; unrelated.
- Choice: “Which supplied speaker explicitly owns this commitment?” Options: speaker IDs plus not-established.
- Choice: “Which source span contains an explicit deadline?” Options: candidate date spans plus none.

### Interpretation and system behavior

Create draft task records only for supported commitments, with source utterances attached. Code resolves dates using the meeting timestamp and asks for confirmation before assigning tasks or sending reminders. In this example, the conditional offer is not an independent assignment.

### Boundaries and failure modes

The pattern does not generate a complete action-item description unless another component writes one. Speaker diarization errors, sarcasm and indirect agreement need review. A due date mentioned elsewhere may belong to a different task.

### How to evaluate

Test false task creation, wrong ownership, conditional promises, reassigned work and ambiguous relative dates. Measure confirmation edits rather than only label agreement.

### Sources and related cases

[Pre-parsed value extraction](https://docs.typesafe.ai/cookbooks/pre_parsed_value_extraction_cookbook) · [Date extraction](https://docs.typesafe.ai/cookbooks/date_extraction_cookbook)

Related: [UC-DD-01](data-and-documents.md#uc-dd-01) · [UC-DD-02](data-and-documents.md#uc-dd-02).


---

<a id="uc-op-04"></a>

## UC-OP-04 — Route invoice and purchase-order exceptions

**Tags:** Choice; Noul; document-matching; exception-routing

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

Accounts-payable staff need help distinguishing semantic description mismatches from exact accounting inconsistencies. Jev can assist the former without calculating tax or authorizing payment.

### Concrete input example

`invoice_line`: “Annual platform access, operations workspace.” `purchase_order_line`: “Operations software subscription, 12-month term.” Supply vendor identifiers, service dates and line descriptions. Code has already compared currencies, quantities and amounts.

### Questions to ask

- Choice: “How do these descriptions relate?” Options: same described deliverable; potentially related but scope differs; different deliverables; insufficient description.
- Noul: “Is there an explicit scope difference that requires purchaser review?”

### Interpretation and system behavior

Code preserves arithmetic exceptions regardless of semantic similarity. A probable description match can attach supporting evidence to the matching workbench; scope differences create a reviewer queue. Payment release remains subject to the established approval process.

### Boundaries and failure modes

This is a proposed review aid, not accounting or tax advice. Semantic equivalence cannot establish that goods were delivered, that bank details are legitimate or that an invoice is payable. Never let a similarity score override a hard amount mismatch.

### How to evaluate

Test partial deliveries, different service periods, bundled line items and changed vendor payment details. Measure false matches and reviewer time, with independent financial controls unchanged.

### Sources and related cases

[Pre-parsed value extraction](https://docs.typesafe.ai/cookbooks/pre_parsed_value_extraction_cookbook) · [Knowledge-graph entity alignment](https://docs.typesafe.ai/cookbooks/entity_alignment)

Related: [UC-DD-01](data-and-documents.md#uc-dd-01) · [UC-DD-04](data-and-documents.md#uc-dd-04).


---

<a id="uc-op-05"></a>

## UC-OP-05 — Dispatch maintenance requests using described symptoms

**Tags:** Choice; Noul; dispatch; maintenance

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A facilities desk receives unstructured reports and needs the appropriate specialist queue. This is dispatch support, not autonomous technical diagnosis or equipment control.

### Concrete input example

`report`: “The meeting-room air unit rattles when the fan starts; there is no visible leak.” Include location, asset inventory, reporter observations and separate verified alarms. Available queues include ventilation, electrical, plumbing and general inspection.

### Questions to ask

- Choice: “Which queue is appropriate for the reported symptoms?” Include uncertain-needs-inspection.
- Noul: “Does the report explicitly describe a condition listed in the organization’s immediate-escalation policy?”
- Choice: “Is the reported symptom ongoing, intermittent, historical or unclear?”

### Interpretation and system behavior

Code uses the asset registry and operating policy to select an eligible queue and attaches the original report. Exact alarm rules bypass semantic inference. For ambiguous cases, the system requests inspection rather than guessing a repair procedure.

### Boundaries and failure modes

Absence of a reported hazard is not evidence of safety. The model cannot inspect equipment from text, and a queue selection should not stop alarms, energize machinery or prescribe hazardous work. Escalation policy must be supplied and owned by the operator.

### How to evaluate

Test mixed symptoms, wrong asset names, incomplete locations and serious reports phrased casually. Measure queue accuracy, lost escalations and the rate of human corrections.

### Sources and related cases

[Intent routing](https://docs.typesafe.ai/patterns/intent-routing) · [Composite scoring](https://docs.typesafe.ai/patterns/composite-scoring)

Related: [UC-OP-01](operations-and-support.md#uc-op-01) · [UC-SP-06](specialist-review.md#uc-sp-06).


---

<a id="uc-op-06"></a>

## UC-OP-06 — Route return requests to the right evidence workflow

**Tags:** Choice; Noul; returns; evidence-routing

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A merchant needs to distinguish damaged-on-arrival, wrong-item and preference-based return requests so the right evidence is collected.

### Concrete input example

`message`: “The parcel contained the smaller adapter, but my order shows the larger version.” Supply verified order lines, shipment identifiers, approved return-workflow descriptions and any customer-provided evidence as separate records.

### Questions to ask

- Choice: “Which return issue is described?” Options: wrong item; damaged item; missing item; preference change; unclear.
- Noul: “Does the supplied evidence identify which order line is affected?”
- Choice: “Which approved evidence step is still needed?” Options describe relevant workflow steps plus no-additional-step-established.

### Interpretation and system behavior

Code routes the case to the relevant evidence checklist, prevents duplicate return authorizations and asks for missing identifiers. It does not decide a refund amount from the customer’s wording. Existing return-policy checks and staff approval remain in control.

### Boundaries and failure modes

This application classifies what is claimed, not whether a customer is honest or legally entitled to a remedy. Do not infer fraud from unusual language. Avoid asking for evidence the system already possesses.

### How to evaluate

Test multiple-item orders, duplicate requests, incomplete receipts, mixed damage/wrong-item reports and paraphrases of the same issue. Evaluate incorrect workflow burdens as well as queue accuracy.

### Sources and related cases

[Intent routing](https://docs.typesafe.ai/patterns/intent-routing) · [Confidence-gated routing](https://docs.typesafe.ai/patterns/confidence-routing)

Related: [UC-OP-01](operations-and-support.md#uc-op-01) · [UC-OP-04](operations-and-support.md#uc-op-04).


[Back to all use cases](index.md) · [Handbook index](../index.md)
