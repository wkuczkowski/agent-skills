# Data and documents

[Handbook index](../index.md) · [All use cases](index.md) · [Evidence labels](index.md#evidence-labels)

Extractive transformations, entity linking and taxonomies.

Research reviewed: 2026-09-21. All concrete inputs are hypothetical; no numeric model outputs or performance results are asserted. Question lists specify meaning, not a copy-paste API payload. Suggested operating controls are design adaptations, not claims about a linked project’s exact implementation. Apply the shared [decision semantics](../decision-design.md) and [evaluation controls](../evaluation-and-operations.md).

## On this page


- [UC-DD-01 — Select a source value from parser-generated candidates](#uc-dd-01) · **W**

- [UC-DD-02 — Interpret date expressions and resolve them in code](#uc-dd-02) · **W**

- [UC-DD-03 — Recover document structure without rewriting the text](#uc-dd-03) · **W**

- [UC-DD-04 — Link candidate records that refer to the same entity](#uc-dd-04) · **W**

- [UC-DD-05 — Classify into a hierarchy and stop at defensible specificity](#uc-dd-05) · **W**

- [UC-DD-06 — Extract a relation by selecting evidence and a defined relation type](#uc-dd-06) · **P**


---

<a id="uc-dd-01"></a>

## UC-DD-01 — Select a source value from parser-generated candidates

**Tags:** Choice; Noul; span-selection; extraction

**Evidence basis — W:** Official worked pattern; the concrete scenario and operating policy here are illustrative adaptations, not reproduced benchmark results.

### Problem and Jev’s role

A document contains multiple valid-looking emails, phone numbers or amounts. Parsing finds strings; semantic selection identifies which one has the requested role.

### Concrete input example

`text`: “Old billing contact: old@example.org. Send future invoices to accounts@example.org. Total 480.00; already paid 180.00.” `candidates`: each exact matched span with offsets and surrounding text. The desired field is the current invoice-recipient email, not any email.

### Questions to ask

- Choice: “Which candidate is explicitly designated for future invoice delivery?” Options: candidate IDs with their context, plus not-stated and ambiguous.
- Noul: “Does the document indicate a change in the invoice-recipient role from a previous value?” This is optional when change tracking is useful.

### Interpretation and system behavior

Code copies the selected span verbatim and normalizes only under explicit rules. It stores provenance and marks the old contact as historical instead of silently overwriting without an audit trail. Amount arithmetic, syntax validation and database updates remain ordinary code.

### Boundaries and failure modes

Parser recall is an upper bound on extractive success. A correctly formatted value can have the wrong role. Do not use Score to interpolate an exact amount or ask the model to invent a missing candidate.

### How to evaluate

Measure end-to-end field accuracy and candidate recall separately. Include split email strings, multiple locales, forwarding headers, historical values and missing requested fields.

### Sources and related cases

[Pre-parsed value extraction](https://docs.typesafe.ai/cookbooks/pre_parsed_value_extraction_cookbook)

Related: [UC-AG-05](agents-and-routing.md#uc-ag-05) · [UC-DD-02](data-and-documents.md#uc-dd-02).


---

<a id="uc-dd-02"></a>

## UC-DD-02 — Interpret date expressions and resolve them in code

**Tags:** Choice; temporal-interpretation; calendar-validation

**Evidence basis — W:** Official worked pattern; the concrete scenario and operating policy here are illustrative adaptations, not reproduced benchmark results.

### Problem and Jev’s role

Free text describes a date using calendar words or relative expressions. The semantic task is to identify the expression’s meaning, not perform authoritative calendar arithmetic.

### Concrete input example

`text`: “Please schedule the inspection next Friday afternoon.” Supply the message timestamp, business timezone, locale, candidate spans and whether a scheduling convention for “next Friday” has already been agreed. The target slot inventory is a separate source.

### Questions to ask

- Choice: “What kind of date expression is used?” Options: explicit calendar date; weekday-relative date; duration from a reference; ambiguous or absent.
- Choice: “Which weekday is explicitly requested?” Options: named weekdays plus not-stated.
- Choice: “What time-of-day category is requested?” Options describe allowed business periods plus unspecified.

### Interpretation and system behavior

Code resolves the calendar date against the authoritative timestamp and timezone, validates it, and looks up actual availability. Ambiguous interpretations require a clarification or a displayed confirmation. The selected date is not itself a booking authorization.

### Boundaries and failure modes

Independent date components can produce an impossible combination; validate the assembled date. Do not encode a universal interpretation of “next Friday” or silently assume a timezone. Relative expressions tied to another event require that event’s actual timestamp.

### How to evaluate

Test month and year boundaries, leap days, daylight-saving transitions, late-arriving messages, ambiguous numeric formats and dates that are valid but unavailable.

### Sources and related cases

[Date extraction](https://docs.typesafe.ai/cookbooks/date_extraction_cookbook)

Related: [UC-AG-02](agents-and-routing.md#uc-ag-02) · [UC-OP-03](operations-and-support.md#uc-op-03).


---

<a id="uc-dd-03"></a>

## UC-DD-03 — Recover document structure without rewriting the text

**Tags:** Noul; Choice; extractive-transformation; formatting

**Evidence basis — W:** Official worked pattern; the concrete scenario and operating policy here are illustrative adaptations, not reproduced benchmark results.

### Problem and Jev’s role

A text export has lost paragraph and heading structure. Jev can help identify boundaries and block roles while code preserves the source wording.

### Concrete input example

`lines`: indexed text from a plain-text export, including hard-wrapped paragraphs, headings, bullets and code-like examples. Preserve blank lines, original offsets and extraction warnings. No OCR capability is implied by this step.

### Questions to ask

- Noul per adjacent line pair: “Do these lines belong to the same prose paragraph rather than separate structural blocks?”
- Second-stage Choice per rebuilt block: “What is this block’s role?” Options: heading, paragraph, list item, code, callout, unclear.
- Speculative Choice: “Assuming this block is a heading, which supported level fits the surrounding hierarchy?”

### Interpretation and system behavior

Code first assembles blocks, then requests classifications using the new structure. It renders Markdown from templates and retains an offset map to the original text. Unclear regions remain plain text; the process does not add explanatory sentences.

### Boundaries and failure modes

Joining lines can corrupt code or tables. A formatting classifier is not a repair for missing extraction data. Make all transformations reversible and distinguish layout recovery from semantic editing or summarization.

### How to evaluate

Test round-trip preservation of words and identifiers, heading nesting, code whitespace, nested lists and footnotes. Review a rendered sample as well as the text diff.

### Sources and related cases

[Structure recovery](https://docs.typesafe.ai/cookbooks/autoformat)

Related: [UC-DD-01](data-and-documents.md#uc-dd-01) · [UC-SR-02](search-and-knowledge.md#uc-sr-02).


---

<a id="uc-dd-04"></a>

## UC-DD-04 — Link candidate records that refer to the same entity

**Tags:** Score; Noul; entity-resolution; record-linkage

**Evidence basis — W:** Official worked pattern; the concrete scenario and operating policy here are illustrative adaptations, not reproduced benchmark results.

### Problem and Jev’s role

Two datasets describe overlapping entities with inconsistent names. Jev can assess candidate pairs, while a separate process enforces entity-resolution rules.

### Concrete input example

`left`: “Aster mug, blue, 350 ml, SKU A-350-B.” `right`: “Aster ceramic cup 0.35 L blue, manufacturer code A-350-B.” Include source IDs, variants, timestamps and provenance. Code normalizes units and proposes candidate pairs before semantic comparison.

### Questions to ask

- Score: “How strongly does the supplied evidence support identity of the same sellable variant?” Levels: clearly different entities; plausible relation but identity unresolved; evidence consistently identifies the same variant.
- Noul: “Is there an explicit conflicting identifier or variant attribute in the pair?”

### Interpretation and system behavior

Code uses the distribution, conflict indicators and hard identifier rules to leave unlinked, suggest a link or queue review. Preserve original rows and make accepted links reversible. In a connected graph, inspect the consistency of a whole cluster before merging it.

### Boundaries and failure modes

The expected Score’s middle value can represent uncertainty between extremes rather than a genuine middle judgment. Same brand or product family is not same variant. A local example policy is not a universal safe auto-merge threshold.

### How to evaluate

Evaluate false merges separately from missed links, including transitive cluster errors. Test reused identifiers, unit normalization, packaging differences and subtly distinct variants.

### Sources and related cases

[Knowledge-graph entity alignment](https://docs.typesafe.ai/cookbooks/entity_alignment) · [Score](https://docs.typesafe.ai/primitives/score)

Related: [UC-DD-05](data-and-documents.md#uc-dd-05) · [UC-OP-02](operations-and-support.md#uc-op-02).


---

<a id="uc-dd-05"></a>

## UC-DD-05 — Classify into a hierarchy and stop at defensible specificity

**Tags:** Choice; Noul; hierarchy; coarse-fallback

**Evidence basis — W:** Official worked pattern; the concrete scenario and operating policy here are illustrative adaptations, not reproduced benchmark results.

### Problem and Jev’s role

A large catalog or document taxonomy is too broad for one unwieldy label list. Classification can traverse a versioned hierarchy and abstain from overly specific leaves.

### Concrete input example

`item`: “Water-resistant trail running shoe with removable insole.” `taxonomy`: category IDs, parent-child relationships and inclusion/exclusion definitions. Include the current branch description in each node assessment rather than relying on opaque labels.

### Questions to ask

- Choice at a node: “Which child category best matches the described item under this taxonomy?” Include none-of-these and insufficient-description.
- Noul: “Does the supplied description establish the distinguishing property needed for this proposed leaf?”

### Interpretation and system behavior

Code explores a small number of plausible branches, fetches the next node definitions and records candidate paths. It reports a broader category only when evidence supports that parent, not simply the parent of a low-confidence winner. Unresolved cross-branch cases remain unresolved.

### Boundaries and failure modes

Path scores assembled from local probabilities are ranking heuristics, not automatically calibrated probabilities of final correctness. Taxonomy revisions can change meaning even when display names stay the same. Avoid hallucinating attributes to satisfy a leaf.

### How to evaluate

Measure accuracy by depth, wrong-branch errors and useful coarse coverage. Test ambiguous hybrids, missing attributes, taxonomy migrations and cases with no valid leaf.

### Sources and related cases

[Hierarchical classification](https://docs.typesafe.ai/cookbooks/hierarchical_classification) · [Classification using confidence](https://docs.typesafe.ai/cookbooks/classification_using_confidence)

Related: [UC-CC-01](commerce-and-content.md#uc-cc-01) · [UC-SW-02](software-and-security.md#uc-sw-02).


---

<a id="uc-dd-06"></a>

## UC-DD-06 — Extract a relation by selecting evidence and a defined relation type

**Tags:** Choice; Noul; relation-extraction; knowledge-graph

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A knowledge graph needs explicit, source-backed relationships rather than unconstrained generated triples.

### Concrete input example

`text`: “Helio Labs distributes devices made by Northstar. It acquired the Orion service brand.” Code provides candidate entity mentions, spans and candidate pairs. Desired relations are a defined set such as manufactures, distributes, acquired and no-stated-relation.

### Questions to ask

- Choice per pair: “Which listed relationship is explicitly stated between these two entities in this source?” Include ambiguous and no-stated-relation.
- Choice: “Which supplied sentence directly supports the relationship?” Options are source sentence IDs plus none.
- Noul: “Does the sentence describe a completed relationship rather than a proposal, denial or hypothetical?”

### Interpretation and system behavior

Code writes only an evidence-backed candidate edge, retaining subject/object direction, qualifiers and source version. Human review or a separate policy controls promotion to the authoritative graph. Pronouns that cannot be resolved from the supplied context remain unresolved.

### Boundaries and failure modes

This is a proposed extractive design. Mentioning two entities together does not establish a relationship. No-stated-relation is not a claim that no real-world relationship exists, and a quoted allegation is not automatically an established fact.

### How to evaluate

Test relation direction, negation, proposed acquisitions, historical relationships, ambiguous pronouns and multiple entities in one sentence. Measure supported-edge precision and provenance integrity.

### Sources and related cases

[Pre-parsed value extraction](https://docs.typesafe.ai/cookbooks/pre_parsed_value_extraction_cookbook) · [Choice](https://docs.typesafe.ai/primitives/choice)

Related: [UC-SR-06](search-and-knowledge.md#uc-sr-06) · [UC-DD-04](data-and-documents.md#uc-dd-04).


[Back to all use cases](index.md) · [Handbook index](../index.md)
