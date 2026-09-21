# Specialist review workflows

[Handbook index](../index.md) · [All use cases](index.md) · [Evidence labels](index.md#evidence-labels)

Evidence preparation for qualified reviewers, not autonomous consequential decisions.

Research reviewed: 2026-09-21. All concrete inputs are hypothetical; no numeric model outputs or performance results are asserted. Question lists specify meaning, not a copy-paste API payload. Suggested operating controls are design adaptations, not claims about a linked project’s exact implementation. Apply the shared [decision semantics](../decision-design.md) and [evaluation controls](../evaluation-and-operations.md).

## On this page


- [UC-SP-01 — Prepare contract-clause deviations for legal review](#uc-sp-01) · **P**

- [UC-SP-02 — Organize an insurance claim’s evidence packet](#uc-sp-02) · **P**

- [UC-SP-03 — Locate explicit skill evidence for a recruiting reviewer](#uc-sp-03) · **P**

- [UC-SP-04 — Check administrative completeness of a clinical document packet](#uc-sp-04) · **P**

- [UC-SP-05 — Route financial anomaly narratives to a review category](#uc-sp-05) · **P**

- [UC-SP-06 — Code technician observations for quality-review workflows](#uc-sp-06) · **P**


---

<a id="uc-sp-01"></a>

## UC-SP-01 — Prepare contract-clause deviations for legal review

**Tags:** Choice; Noul; legal-review-support; evidence-packets

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A legal reviewer has an approved clause playbook and needs to find passages that differ from it. Jev can organize evidence; a qualified professional determines legal meaning and appropriate action.

### Concrete input example

`playbook`: a supplied, versioned clause requirement and exceptions. `clause`: candidate contract text with section IDs, definitions and neighboring qualifications. Example: compare a renewal-notice clause with the organization’s stated drafting preference.

### Questions to ask

- Choice: “How does this clause relate to the supplied playbook requirement?” Options: expressly matches; expressly differs; depends on another referenced provision; insufficient context.
- Choice: “Which supplied span contains the relevant qualification or deviation?” Include none.
- Noul: “Does the clause refer to a definition or schedule absent from the supplied materials?”

### Interpretation and system behavior

Create a review packet with the exact clause, comparison category and missing cross-references. Code checks document version and preserves source pagination. A lawyer resolves interpretation, current-law questions and drafting changes.

### Boundaries and failure modes

This is a proposed document-review aid, not legal advice or a legal compliance determination. Do not equate playbook conformity with enforceability. No jurisdictional rule is supplied by this handbook; relevant law must be researched separately.

### How to evaluate

Test exceptions, incorporated schedules, defined terms and amendments that supersede the base document. Measure missed material deviations and evidence-location accuracy using expert-reviewed examples.

### Sources and related cases

[Double-checking citations](https://docs.typesafe.ai/cookbooks/citation_check) · [Pre-parsed value extraction](https://docs.typesafe.ai/cookbooks/pre_parsed_value_extraction_cookbook)

Related: [UC-SR-04](search-and-knowledge.md#uc-sr-04) · [UC-DD-06](data-and-documents.md#uc-dd-06).


---

<a id="uc-sp-02"></a>

## UC-SP-02 — Organize an insurance claim’s evidence packet

**Tags:** Choice; insurance-review-support; evidence-completeness

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

Claims staff need an organized account of what submitted documents state and what is missing. The system should not autonomously determine entitlement, payment or denial.

### Concrete input example

`checklist`: evidence requested by an authorized claim handler. `documents`: a claimant statement, repair estimate and dated photographs described by a separate extraction system. Keep allegations, estimates and independently verified facts distinct.

### Questions to ask

- Choice per checklist item: “What evidence status is established by the submitted materials?” Options: explicit supporting document present; relevant but incomplete; contradictory materials; not supplied.
- Choice: “Which supplied document supports that status?” Options: document IDs plus none.

### Interpretation and system behavior

Code assembles a packet with document references and a missing-information list for staff review. Exact policy dates and amounts are calculated or checked by ordinary systems. A reviewer decides whether any additional information is necessary and what action is appropriate.

### Boundaries and failure modes

The official consistency example informs uncertainty handling, not this workflow’s eligibility rules. Lack of evidence is not evidence of dishonesty or ineligibility. Neither a confident label nor repeated agreement authorizes an adverse claim decision.

### How to evaluate

Test conflicting dates, duplicate uploads, unreadable attachments and evidence spread across documents. Measure missed supporting evidence and unnecessary information requests, with human adjudication unchanged.

### Sources and related cases

[Self-consistency: Noul](https://docs.typesafe.ai/cookbooks/consistency_noul_cookbook) · [Pre-parsed value extraction](https://docs.typesafe.ai/cookbooks/pre_parsed_value_extraction_cookbook)

Related: [UC-OP-06](operations-and-support.md#uc-op-06) · [UC-DD-01](data-and-documents.md#uc-dd-01).


---

<a id="uc-sp-03"></a>

## UC-SP-03 — Locate explicit skill evidence for a recruiting reviewer

**Tags:** Choice; recruiting-review-support; evidence-extraction

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A recruiting team needs to find evidence relevant to a job’s stated requirements without asking the model to rank people or infer personal characteristics.

### Concrete input example

`requirement`: “Experience maintaining a production SQL database.” `application`: candidate-provided role descriptions and portfolio excerpts. Remove irrelevant sensitive information where feasible and supply the exact requirement, not a vague “culture fit” concept.

### Questions to ask

- Choice: “What does the supplied material explicitly establish about this requirement?” Options: directly described experience; related but not equivalent experience; not stated; conflicting information.
- Choice: “Which supplied passage is the supporting evidence?” Include no-passage.

### Interpretation and system behavior

Produce an evidence table for a human reviewer, with gaps represented as not-stated rather than not-capable. The reviewer may ask a consistent follow-up question or inspect the portfolio. The system does not score overall suitability or make hiring decisions.

### Boundaries and failure modes

This is a proposed evidence-location tool, not a validated employment assessment. Resume omission is not lack of ability. Do not infer age, ethnicity, disability, personality or other sensitive characteristics from names, style or background.

### How to evaluate

Test equivalent evidence written in different styles and languages, career changes, gaps and nontraditional experience. Evaluate evidence recall and false assertions, not a model-generated ranking of applicants.

### Sources and related cases

[Pre-parsed value extraction](https://docs.typesafe.ai/cookbooks/pre_parsed_value_extraction_cookbook) · [Noul](https://docs.typesafe.ai/primitives/noul)

Related: [UC-SR-02](search-and-knowledge.md#uc-sr-02) · [UC-RA-03](research-and-analytics.md#uc-ra-03).


---

<a id="uc-sp-04"></a>

## UC-SP-04 — Check administrative completeness of a clinical document packet

**Tags:** Choice; administrative-healthcare; completeness-check

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A clinical administrator needs to identify missing forms or document sections before a qualified professional reviews a packet. This is not diagnosis, treatment advice or clinical urgency triage.

### Concrete input example

`required_packet`: an institution-approved administrative checklist. `documents`: referral text, attached reports and consent-form metadata, with dates and provenance. Use only information the organization is authorized to process through the chosen provider.

### Questions to ask

- Choice per checklist element: “What does the supplied packet establish?” Options: required document or section present; present but incomplete; contradictory metadata; not provided.
- Choice: “Which supplied document contains this administrative element?” Include none.

### Interpretation and system behavior

Code checks exact identifiers and dates, generates a staff-facing completeness report and preserves links to source documents. A professional decides whether missing items matter clinically and how the case should proceed. Existing clinical escalation protocols are not replaced.

### Boundaries and failure modes

The proposed workflow must not delay urgent care based on a model’s paperwork assessment. Missing data does not establish a patient condition. Sensitive-data processing, access controls and current agreements require separate review.

### How to evaluate

Test partial scans, misfiled attachments, mismatched patient identifiers and completed forms with missing sections. Measure false missing-item flags and overlooked documents against staff review.

### Sources and related cases

[Pre-parsed value extraction](https://docs.typesafe.ai/cookbooks/pre_parsed_value_extraction_cookbook) · [Choice](https://docs.typesafe.ai/primitives/choice)

Related: [UC-DD-01](data-and-documents.md#uc-dd-01) · [UC-SP-02](specialist-review.md#uc-sp-02).


---

<a id="uc-sp-05"></a>

## UC-SP-05 — Route financial anomaly narratives to a review category

**Tags:** Choice; Noul; financial-review-support; triage

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A financial operations team has deterministic alerts and needs to organize the accompanying narrative for an analyst. Jev should not label a person as fraudulent or decide access to funds.

### Concrete input example

`alert`: a rules-engine notification. `narrative`: a customer explanation of a duplicate transfer. Include verified transaction references and timestamps separately from the narrative. The review taxonomy distinguishes processing error, duplicate instruction, disputed recipient and unclear explanation.

### Questions to ask

- Choice: “Which review category best describes the explanation supplied?” Include multiple-categories and unclear.
- Noul: “Does the explanation reference a specific supporting document that is missing from the packet?”
- Choice: “Which supplied statement needs comparison with verified transaction data?” Options: statement IDs plus none.

### Interpretation and system behavior

Code routes the packet and attaches relevant verified records. Analysts compare claims with authoritative data and apply the organization’s procedures. Model output is stored as a triage signal, not a finding of wrongdoing.

### Boundaries and failure modes

This is a proposed administrative use, not fraud detection with demonstrated accuracy, investment advice or an automated adverse-decision system. Tone, grammar or nationality should not become proxies for dishonesty. Hard security and transaction rules remain independent.

### How to evaluate

Test innocent duplicates, language variation, incomplete explanations and conflicting records. Measure useful routing, missing-evidence detection and analyst corrections, not accusations generated.

### Sources and related cases

[Intent routing](https://docs.typesafe.ai/patterns/intent-routing) · [Noul](https://docs.typesafe.ai/primitives/noul)

Related: [UC-OP-04](operations-and-support.md#uc-op-04) · [UC-SW-03](software-and-security.md#uc-sw-03).


---

<a id="uc-sp-06"></a>

## UC-SP-06 — Code technician observations for quality-review workflows

**Tags:** Noul; Choice; quality-review; observation-coding

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A quality team wants structured categories from technician notes so recurring issues can be analyzed. The model does not replace measurements, inspection equipment or product-release authorization.

### Concrete input example

`note`: “Outer carton dented; protective foam intact; connector latch does not hold on the test fixture.” Supply the inspection checklist, measured results and product revision. Keep packaging observations separate from functional test outcomes.

### Questions to ask

- Noul per defect category: “Does the note explicitly describe the defined observation?” Categories might distinguish packaging damage, cosmetic damage and functional retention failure.
- Choice: “Is this observation a direct test result, a visual observation, a suspected cause or an unclear statement?”
- Choice: “Which supplied span supports the functional issue category?” Include none.

### Interpretation and system behavior

Code creates categorized observations with source references and aggregates them by batch or revision. Existing measurement limits and release gates remain decisive. Engineers review causes and decide corrective action; a semantic label alone does not stop or release a production batch.

### Boundaries and failure modes

This proposed pattern describes recorded evidence, not whether a product is safe. A technician’s causal guess must not be promoted to a measured fact. Missing measurement values require proper inspection rather than model inference.

### How to evaluate

Test mixed packaging/function notes, negated defects, copied templates and observations about accessories rather than the main item. Measure category precision and missed functional issues using independent inspection records.

### Sources and related cases

[Composite scoring](https://docs.typesafe.ai/patterns/composite-scoring) · [Noul](https://docs.typesafe.ai/primitives/noul)

Related: [UC-OP-05](operations-and-support.md#uc-op-05) · [UC-RA-04](research-and-analytics.md#uc-ra-04).


[Back to all use cases](index.md) · [Handbook index](../index.md)
