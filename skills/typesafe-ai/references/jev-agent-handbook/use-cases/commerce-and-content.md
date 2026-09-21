# Commerce and content

[Handbook index](../index.md) · [All use cases](index.md) · [Evidence labels](index.md#evidence-labels)

Catalogs, matching, editorial review and feedback.

Research reviewed: 2026-09-21. All concrete inputs are hypothetical; no numeric model outputs or performance results are asserted. Question lists specify meaning, not a copy-paste API payload. Suggested operating controls are design adaptations, not claims about a linked project’s exact implementation. Apply the shared [decision semantics](../decision-design.md) and [evaluation controls](../evaluation-and-operations.md).

## On this page


- [UC-CC-01 — Tag product attributes from explicit descriptions](#uc-cc-01) · **P**

- [UC-CC-02 — Match products against explicit user constraints](#uc-cc-02) · **P**

- [UC-CC-03 — Detect inconsistency between listing text and structured fields](#uc-cc-03) · **P**

- [UC-CC-04 — Assist content-moderation review under an explicit policy](#uc-cc-04) · **P**

- [UC-CC-05 — Score drafts against an editorial brief](#uc-cc-05) · **P**

- [UC-CC-06 — Code customer feedback by topic and expressed stance](#uc-cc-06) · **P**


---

<a id="uc-cc-01"></a>

## UC-CC-01 — Tag product attributes from explicit descriptions

**Tags:** Noul; Choice; multi-label; product-attributes

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A catalog needs multiple searchable attributes. Independent labels are a better conceptual fit than forcing every item into one mutually exclusive class.

### Concrete input example

`description`: “Packable waterproof cycling jacket with reflective strips; no insulated lining.” `attribute_definitions`: waterproof, reflective, insulated and packable, with evidence requirements. Keep manufacturer specifications distinct from seller marketing.

### Questions to ask

- Noul per attribute: “Does the supplied description explicitly support this attribute under its definition?”
- Choice for important missing fields: “What is the evidence status?” Options: explicitly present; explicitly absent; not stated; conflicting sources.

### Interpretation and system behavior

Store labels with supporting source spans or document references. Use explicit-absence and unknown states rather than representing every missing label as false. Exact measurements and unit conversions come from parsers or structured fields.

### Boundaries and failure modes

This is a proposed tagging system. A marketing adjective may not satisfy a technical specification. Independently inferred attributes can conflict and require schema rules; model output should not create a certification claim that the source never made.

### How to evaluate

Test negation, compatibility claims, accessories described alongside a main product and conflicting seller/manufacturer text. Measure per-label precision, missing-data handling and search usefulness.

### Sources and related cases

[Hierarchical classification](https://docs.typesafe.ai/cookbooks/hierarchical_classification) · [Noul](https://docs.typesafe.ai/primitives/noul)

Related: [UC-DD-05](data-and-documents.md#uc-dd-05) · [UC-CC-03](commerce-and-content.md#uc-cc-03).


---

<a id="uc-cc-02"></a>

## UC-CC-02 — Match products against explicit user constraints

**Tags:** Noul; Score; constrained-matching; preferences

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A product finder should separate non-negotiable constraints from preferences and rank only genuinely eligible options.

### Concrete input example

`request`: “A backpack under my stated budget, with a separate laptop compartment; low weight is preferable.” `candidate`: verified price, inventory, dimensions and descriptive features. Currency and budget comparisons are calculated in code.

### Questions to ask

- Noul: “Does the supplied product evidence establish a separate laptop compartment, rather than a general internal pocket?”
- Score: “How directly do the described features support the user’s commuting preference?” Levels: no relevant features; some useful features; several directly relevant features.
- Choice: “Is evidence for the required feature explicit, conflicting or missing?”

### Interpretation and system behavior

Code removes budget and availability failures, preserves unknown required features for clarification and combines only preference scores. The user sees a shortlist with source-backed reasons, not invented specifications. A separate writer may summarize those reasons.

### Boundaries and failure modes

A favorable preference score cannot compensate for a failed required feature. Current prices and inventory must come from live commerce data, not the model. Reweighting preferences may reuse judgments only when the evidence and rubric are unchanged.

### How to evaluate

Test hard-constraint violations, ambiguous feature wording, stale stock, unit differences and adversarial promotional language. Measure eligible-shortlist precision before ranking satisfaction.

### Sources and related cases

[Composite scoring](https://docs.typesafe.ai/patterns/composite-scoring) · [Candidate reranking](https://docs.typesafe.ai/cookbooks/rerank_typesafe)

Related: [UC-CC-01](commerce-and-content.md#uc-cc-01) · [UC-SR-01](search-and-knowledge.md#uc-sr-01).


---

<a id="uc-cc-03"></a>

## UC-CC-03 — Detect inconsistency between listing text and structured fields

**Tags:** Choice; consistency-check; catalog-quality

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A marketplace wants to review listings whose title or prose conflicts with structured attributes, without rewriting the seller’s content automatically.

### Concrete input example

`title`: “Twin-pack reusable filter.” `description`: “One replacement cartridge supplied.” `fields`: package quantity two. Supply the relevant source sections, unit definitions and any manufacturer record.

### Questions to ask

- Choice: “How do the supplied statements about package contents relate?” Options: consistent; directly conflicting; different quantities refer to different objects; insufficient context.
- Choice: “Which supplied span is the source of the conflicting claim?” Options: span IDs plus none.

### Interpretation and system behavior

Create a targeted review item showing the conflicting fields and unchanged source text. Code handles arithmetic and exact quantity comparisons after semantic role identification. A reviewer chooses which source to correct and preserves the edit history.

### Boundaries and failure modes

The design does not establish the true package contents from contradictory text. “Two layers” and “two units” are not the same quantity, and accessory counts can be legitimate. Do not auto-unpublish based on one unvalidated judgment.

### How to evaluate

Test bundle quantities, accessories, unit-versus-package distinctions, copied descriptions and intentional comparison tables. Measure useful contradiction precision and unnecessary seller interventions.

### Sources and related cases

[Double-checking citations](https://docs.typesafe.ai/cookbooks/citation_check) · [Pre-parsed value extraction](https://docs.typesafe.ai/cookbooks/pre_parsed_value_extraction_cookbook)

Related: [UC-SR-06](search-and-knowledge.md#uc-sr-06) · [UC-DD-01](data-and-documents.md#uc-dd-01).


---

<a id="uc-cc-04"></a>

## UC-CC-04 — Assist content-moderation review under an explicit policy

**Tags:** Noul; Choice; Score; moderation; human-review

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A community needs policy-specific content signals and an uncertainty path. The model should not silently substitute an unspecified moral judgment for the platform’s written rules.

### Concrete input example

`content`: a comment that quotes an insult while criticizing harassment. Include the immediate conversation context, the policy version and definitions of the relevant categories. Keep user-history enforcement rules separate from content interpretation.

### Questions to ask

- Noul per policy condition: “Does this content meet the supplied definition of targeted harassment?” Use separate questions for independently applicable conditions.
- Choice: “How is the potentially violating language used?” Options: directed at a person; quoted for discussion; fictional or hypothetical; unclear.
- Score: “What severity is supported under this policy’s described levels?” Define observable levels, not vague numbers.

### Interpretation and system behavior

Code applies the platform’s reviewed moderation policy to the signals and routes uncertain or consequential cases to a moderator. Preserve the source and allow correction of mistaken decisions. Stability checks can reveal boundary sensitivity but do not prove the policy interpretation is correct.

### Boundaries and failure modes

The handbook does not supply a moderation policy or universal threshold. Quotation, satire and dialect can change meaning. No confidence value should automatically justify an irreversible account sanction.

### How to evaluate

Evaluate category-specific false positives and false negatives, contextual quotation cases, language slices and moderator reversals. Report automatic-action coverage separately from accepted-case error.

### Sources and related cases

[Guardrails for LLMs](https://docs.typesafe.ai/cookbooks/llm_guardrails) · [Self-consistency: Choice](https://docs.typesafe.ai/cookbooks/consistency_choice_cookbook)

Related: [UC-SW-04](software-and-security.md#uc-sw-04) · [UC-RA-05](research-and-analytics.md#uc-ra-05).


---

<a id="uc-cc-05"></a>

## UC-CC-05 — Score drafts against an editorial brief

**Tags:** Score; Noul; editorial-review; reusable-signals

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

An editor has several candidate drafts and wants a reusable assessment of observable requirements. Jev can supply dimensions; it does not need to write or rewrite the article.

### Concrete input example

`brief`: explain a product change to existing users, state the required action and avoid unsupported promises. `draft`: candidate text. Supply approved facts and the intended audience; exact word counts and mandatory strings are checked by code.

### Questions to ask

- Score: “How clearly does the draft identify the action the reader should take?” Levels: no action identifiable; action present but incomplete; action and necessary context clearly stated.
- Noul: “Does the draft make a material promise not supported by `approved_facts`?”
- Score: “How well does the explanation assume the audience’s stated prior knowledge?” Use explicit descriptions of mismatched, partly matched and matched detail.

### Interpretation and system behavior

Code rejects mandatory factual failures independently of style preferences, then lets an editor compare drafts using adjustable weights. Preserve each dimension and its evidence context. A generative editor may make revisions, which receive a fresh evaluation.

### Boundaries and failure modes

This is not an objective aesthetic ranking or a proof of truth. Rubrics reflect editorial choices and need reviewer agreement. A polished draft can still omit an important fact; completeness checks require the actual approved fact list.

### How to evaluate

Test unsupported claims hidden in fluent prose, missing actions, overly technical explanations and adversarial rubric repetition. Compare with blinded editor judgments and the number of useful revisions.

### Sources and related cases

[Composite scoring](https://docs.typesafe.ai/patterns/composite-scoring) · [Score](https://docs.typesafe.ai/primitives/score)

Related: [UC-SR-04](search-and-knowledge.md#uc-sr-04) · [UC-RA-03](research-and-analytics.md#uc-ra-03).


---

<a id="uc-cc-06"></a>

## UC-CC-06 — Code customer feedback by topic and expressed stance

**Tags:** Noul; Choice; feedback; topic-sentiment

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

Feedback often praises one feature while criticizing another. A single whole-message sentiment label would obscure the actionable distinction.

### Concrete input example

`feedback`: “Search is much faster, but exporting large files still fails. I would use scheduled exports.” Supply the product’s topic taxonomy and definitions of praise, complaint, request and neutral report.

### Questions to ask

- Noul per topic: “Does this feedback discuss the defined topic?”
- Choice per mentioned topic: “What stance is expressed about this topic?” Options: praise; complaint; feature request; mixed; neutral; not-discussed.
- Choice: “Which supplied span best supports the topic-level stance?” Include no-clear-span.

### Interpretation and system behavior

Code stores topic-level observations, with mixed and absent states kept distinct. Analysts can aggregate by product version and time after deduplicating responses. Exact counts and trend calculations stay outside the model.

### Boundaries and failure modes

Do not infer a customer’s psychological state or future behavior from one complaint. Sarcasm and conflicting views need context. The observed feedback population is not automatically representative of all customers.

### How to evaluate

Test multi-topic comments, sarcasm, historical comparisons, requested features and duplicated campaigns. Measure per-topic agreement and stability of aggregate conclusions under reviewer corrections.

### Sources and related cases

[Noul](https://docs.typesafe.ai/primitives/noul) · [Choice](https://docs.typesafe.ai/primitives/choice)

Related: [UC-RA-03](research-and-analytics.md#uc-ra-03) · [UC-RA-06](research-and-analytics.md#uc-ra-06).


[Back to all use cases](index.md) · [Handbook index](../index.md)
