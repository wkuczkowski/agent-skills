# Research and analytics

[Handbook index](../index.md) · [All use cases](index.md) · [Evidence labels](index.md#evidence-labels)

Screening, coding, semantic features and evaluation data.

Research reviewed: 2026-09-21. All concrete inputs are hypothetical; no numeric model outputs or performance results are asserted. Question lists specify meaning, not a copy-paste API payload. Suggested operating controls are design adaptations, not claims about a linked project’s exact implementation. Apply the shared [decision semantics](../decision-design.md) and [evaluation controls](../evaluation-and-operations.md).

## On this page


- [UC-RA-01 — Screen titles and abstracts for a research review](#uc-ra-01) · **P**

- [UC-RA-02 — Classify the role of an evidence passage](#uc-ra-02) · **P**

- [UC-RA-03 — Apply a qualitative codebook to open-ended responses](#uc-ra-03) · **P**

- [UC-RA-04 — Turn text into features for a separate predictive model](#uc-ra-04) · **W**

- [UC-RA-05 — Select informative cases for human labeling and threshold review](#uc-ra-05) · **P**

- [UC-RA-06 — Monitor semantic drift and audit incoming datasets](#uc-ra-06) · **P**


---

<a id="uc-ra-01"></a>

## UC-RA-01 — Screen titles and abstracts for a research review

**Tags:** Noul; Choice; research-screening; evidence

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A research team wants to prioritize candidate papers for full-text review against a written protocol. Jev can assist screening, not establish study validity or make the final scientific conclusion.

### Concrete input example

`protocol`: a defined population, intervention or phenomenon, eligible designs and date window. `record`: title, abstract, bibliographic metadata and identifier. Code applies exact date and duplicate rules before semantic screening.

### Questions to ask

- Noul per inclusion condition: “Does the abstract provide explicit evidence that the study concerns the protocol’s target population?”
- Choice: “What does the abstract establish about the required study design?” Options: eligible design stated; ineligible design stated; design unclear.
- Choice: “Which supplied sentence supports the screening decision?” Include no-explicit-evidence.

### Interpretation and system behavior

Route unclear records to full-text retrieval rather than treating missing abstract detail as exclusion. Reviewers adjudicate eligibility and inspect the original paper. Keep reason codes and source text so exclusions are auditable.

### Boundaries and failure modes

This is a proposed workflow, not a validated systematic-review screening tool. Abstracts can omit key details. An attractive model probability should not hide false exclusions, and the review protocol must be specified by qualified researchers.

### How to evaluate

Measure recall of eligible studies, especially false exclusions, against independently screened records. Include missing abstracts, multilingual material, secondary analyses and borderline populations.

### Sources and related cases

[Noul](https://docs.typesafe.ai/primitives/noul) · [Hierarchical classification](https://docs.typesafe.ai/cookbooks/hierarchical_classification)

Related: [UC-RA-02](research-and-analytics.md#uc-ra-02) · [UC-SR-02](search-and-knowledge.md#uc-sr-02).


---

<a id="uc-ra-02"></a>

## UC-RA-02 — Classify the role of an evidence passage

**Tags:** Choice; Noul; evidence-classification; research

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A research assistant needs to distinguish actual results from background, hypotheses and limitations before assembling an evidence table.

### Concrete input example

`passage`: “Participants who completed the intervention improved, but the small sample and non-random allocation limit interpretation.” Supply the surrounding section, study ID and the specific claim under examination.

### Questions to ask

- Choice: “What role does this passage play in the document?” Options: observed result; methodological description; author interpretation; stated limitation; background claim; mixed or unclear.
- Noul: “Does the passage explicitly qualify the strength or generalizability of a finding?”
- Choice: “Does the passage support, contradict or fail to establish the target claim as written?”

### Interpretation and system behavior

Code creates an evidence table with verbatim excerpts and separate role/qualification columns. Mixed passages may be segmented before reassessment. Statistical calculations and research conclusions remain with appropriate methods and reviewers.

### Boundaries and failure modes

A passage’s rhetorical role is not a study-quality rating. A reported association is not automatically causal evidence. The model cannot infer missing methods or treat author confidence as methodological strength.

### How to evaluate

Test discussion sections that restate results, quoted prior studies, exploratory findings and limitations that change an apparently strong conclusion. Measure extraction fidelity and reviewer agreement.

### Sources and related cases

[Classifying RAG passages](https://docs.typesafe.ai/cookbooks/classifying_rag_passages) · [Double-checking citations](https://docs.typesafe.ai/cookbooks/citation_check)

Related: [UC-SR-04](search-and-knowledge.md#uc-sr-04) · [UC-RA-01](research-and-analytics.md#uc-ra-01).


---

<a id="uc-ra-03"></a>

## UC-RA-03 — Apply a qualitative codebook to open-ended responses

**Tags:** Noul; Choice; qualitative-coding; surveys

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

An analyst wants consistent initial coding of interview or survey responses while preserving nuance and the option to revise the codebook.

### Concrete input example

`response`: “The tutorial was clear, but I got stuck when importing my old file.” `codebook`: learning clarity, migration difficulty, reliability, missing feature and other, each with definitions and contrast examples. Supply context needed to interpret pronouns.

### Questions to ask

- Noul per code: “Does this response contain evidence for the defined theme?”
- Choice: “Which supplied span best supports the migration-difficulty code?” Include no-supporting-span.
- Noul: “Does the response raise a substantive theme not represented in the current codebook?”

### Interpretation and system behavior

Store multiple supported codes with excerpt references and a codebook version. Analysts review uncertain and novel themes; revisions trigger a controlled recoding decision. Code calculates counts and does not treat model probabilities as fractional human respondents by default.

### Boundaries and failure modes

This proposal is a coding aid, not an objective interpretation of lived experience. A fixed codebook can suppress emerging themes. Preserve original responses and avoid inferring sensitive traits that respondents did not disclose.

### How to evaluate

Compare against multiple human coders, inspect disagreements and measure stability after codebook revisions. Include mixed themes, short answers, irony and culturally specific wording.

### Sources and related cases

[Noul](https://docs.typesafe.ai/primitives/noul) · [Choice](https://docs.typesafe.ai/primitives/choice)

Related: [UC-CC-06](commerce-and-content.md#uc-cc-06) · [UC-RA-05](research-and-analytics.md#uc-ra-05).


---

<a id="uc-ra-04"></a>

## UC-RA-04 — Turn text into features for a separate predictive model

**Tags:** Noul; Score; semantic-features; supervised-ML

**Evidence basis — W:** Official worked pattern; the concrete scenario and operating policy here are illustrative adaptations, not reproduced benchmark results.

### Problem and Jev’s role

A dataset contains useful free-text signals alongside labeled outcomes. Jev can provide semantic features that a conventional supervised model uses; Jev need not predict the final number directly.

### Concrete input example

Illustrative input: maintenance work-order descriptions available when a ticket is opened, paired with later completion duration. Candidate features concern access restrictions, required specialist involvement and whether diagnosis is already known. Exclude text written after the outcome.

### Questions to ask

- Noul: “Does this description explicitly require access outside normal operating hours?”
- Score: “How clearly is the required work specified at ticket opening?” Levels: symptoms only; likely intervention named but incomplete; concrete intervention and prerequisites stated.
- Noul: “Does the record mention a dependency on an external supplier?”

### Interpretation and system behavior

Compute versioned features for training data, fit a supervised model in ordinary ML tooling and evaluate on an untouched test set. A separate generative agent may propose new feature questions, but feature discovery and tuning stay within training/validation data.

### Boundaries and failure modes

The official cookbook demonstrates an iterative feature-discovery pattern; this maintenance dataset is hypothetical. Model-generated features can leak targets, encode spurious proxies or become unstable after model changes. They are not causal explanations.

### How to evaluate

Compare against structured-data-only and simpler text baselines. Use time-aware or entity-separated splits where appropriate, measure predictive error and test feature stability across versions.

### Sources and related cases

[Autoresearch feature discovery](https://docs.typesafe.ai/cookbooks/autoresearch_feature_discovery)

Related: [UC-RA-05](research-and-analytics.md#uc-ra-05) · [UC-RA-06](research-and-analytics.md#uc-ra-06).


---

<a id="uc-ra-05"></a>

## UC-RA-05 — Select informative cases for human labeling and threshold review

**Tags:** uncertainty; active-learning; evaluation; human-review

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A team has a limited review budget. Uncertainty can help choose cases for inspection, but reviewing only uncertain examples would miss confidently wrong predictions.

### Concrete input example

`pool`: predictions, full distributions where available, input metadata, existing labels and model/question versions. Candidate examples include borderline categories, new product types and previously underrepresented languages.

### Questions to ask

- For new annotations, reuse the task’s original Choice, Score or Noul questions rather than asking an unrelated “are you correct?” question.
- Optional Noul: “Does the supplied input lack evidence required by this task definition?” This separates evidence gaps from a boundary between valid labels.

### Interpretation and system behavior

Code builds a mixed sampling policy: uncertain cases, disagreement cases, rare slices and a random audit sample. Human labels update evaluation sets. Threshold tuning uses validation data, and the final test set remains untouched. Keep raw predictions for later policy changes.

### Boundaries and failure modes

This is a proposed review-sampling strategy, not a guarantee of optimal active learning. Distribution concentration is not directly comparable across arbitrary tasks. Repeated agreement is not correctness, and selective sampling biases naive accuracy estimates.

### How to evaluate

Measure errors discovered per review hour and residual accepted-case error on a representative audit sample. Check that rare but confidently misclassified cases are not systematically ignored.

### Sources and related cases

[Confidence](https://docs.typesafe.ai/confidence) · [Self-consistency: Noul](https://docs.typesafe.ai/cookbooks/consistency_noul_cookbook) · [Self-consistency: Choice](https://docs.typesafe.ai/cookbooks/consistency_choice_cookbook) · [scikit-learn: probability calibration](https://scikit-learn.org/stable/modules/calibration.html)

Related: [UC-AG-04](agents-and-routing.md#uc-ag-04) · [UC-RA-06](research-and-analytics.md#uc-ra-06).


---

<a id="uc-ra-06"></a>

## UC-RA-06 — Monitor semantic drift and audit incoming datasets

**Tags:** Choice; Noul; Score; monitoring; dataset-quality

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A text-processing system needs to notice when new inputs no longer resemble the cases on which its behavior was evaluated.

### Concrete input example

`incoming_record`: a support message or document. Supply stable category definitions and an explicit expected input scope. Code tracks source, language, length, schema version and time separately from semantic judgments.

### Questions to ask

- Choice: “Which expected subject area does this record concern?” Include outside-known-scope and unclear.
- Noul: “Does the record contain the information required to apply the existing task rubric?”
- Score: “How directly does the record fit the workflow’s documented input definition?” Levels: outside scope; partial fit; clear fit.

### Interpretation and system behavior

Code aggregates outcomes over time and compares them with evaluated cohorts, preserving sample-size information. A change creates an audit task with representative examples; it does not automatically redefine the taxonomy or retrain production behavior.

### Boundaries and failure modes

Drift indicators are not proof of declining accuracy, and stable distributions do not prove stable quality. A provider outage or parser regression can resemble semantic drift. Separate input changes, judgment changes and service failures.

### How to evaluate

Inject controlled changes in topic mix, extraction quality and language distribution. Measure useful alerts, false alarms and time to identify the actual failure source. Maintain representative labeled audits.

### Sources and related cases

[Composite scoring](https://docs.typesafe.ai/patterns/composite-scoring) · [Parallel questions](https://docs.typesafe.ai/cookbooks/parallel_questions)

Related: [UC-CC-06](commerce-and-content.md#uc-cc-06) · [UC-RA-05](research-and-analytics.md#uc-ra-05).


[Back to all use cases](index.md) · [Handbook index](../index.md)
