# Search and knowledge

[Handbook index](../index.md) · [All use cases](index.md) · [Evidence labels](index.md#evidence-labels)

Retrieval, evidence selection, citations and reuse of existing answers.

Research reviewed: 2026-09-21. All concrete inputs are hypothetical; no numeric model outputs or performance results are asserted. Question lists specify meaning, not a copy-paste API payload. Suggested operating controls are design adaptations, not claims about a linked project’s exact implementation. Apply the shared [decision semantics](../decision-design.md) and [evaluation controls](../evaluation-and-operations.md).

## On this page


- [UC-SR-01 — Rerank a retrieved shortlist](#uc-sr-01) · **W**

- [UC-SR-02 — Find an exact supporting line or passage](#uc-sr-02) · **W**

- [UC-SR-03 — Triage retrieved passages before answer generation](#uc-sr-03) · **W**

- [UC-SR-04 — Check whether a citation supports a claim](#uc-sr-04) · **W**

- [UC-SR-05 — Decide whether a cached answer is reusable](#uc-sr-05) · **P**

- [UC-SR-06 — Surface conflicting claims across sources](#uc-sr-06) · **P**


---

<a id="uc-sr-01"></a>

## UC-SR-01 — Rerank a retrieved shortlist

**Tags:** Score; Noul; reranking; retrieval

**Evidence basis — W:** Official worked pattern; the concrete scenario and operating policy here are illustrative adaptations, not reproduced benchmark results.

### Problem and Jev’s role

A search engine returns candidate passages, products or records. Jev can assess candidates after retrieval; it does not replace the index or recover items excluded by the retriever.

### Concrete input example

`query`: “Instructions for replacing a lost access badge without changing building permissions.” `candidate`: one passage with its title, version and source scope. Build the shortlist using lexical, vector or structured retrieval under the user’s access rights.

### Questions to ask

- Score per candidate: “How well does this passage answer the stated request?” Levels: unrelated; related background but no procedure; partial applicable procedure; directly applicable procedure.
- For an exact evidence-origin task instead, use Noul: “Is this candidate the source passage referenced by the query?” Do not confuse this binary proposition with graded relevance.

### Interpretation and system behavior

Keep the same rubric across candidates, sort in code and retain a no-answer path when all candidates are unsuitable. Apply source-access and version constraints before ranking. A subsequent generative step may explain the selected evidence with citations.

### Boundaries and failure modes

The official cookbook demonstrates a specific binary reranking task; this broader rubric is an adaptation. A Choice distribution over a shortlist is competitive and can change when candidates change. Measure candidate recall separately from reranker quality.

### How to evaluate

Use relevance labels and a held-out query set. Report candidate recall, ranking quality and answer success, including absent answers, near-duplicate passages and obsolete but lexically attractive results.

### Sources and related cases

[Candidate reranking](https://docs.typesafe.ai/cookbooks/rerank_typesafe) · [Score](https://docs.typesafe.ai/primitives/score)

Related: [UC-SR-02](search-and-knowledge.md#uc-sr-02) · [UC-SR-03](search-and-knowledge.md#uc-sr-03).


---

<a id="uc-sr-02"></a>

## UC-SR-02 — Find an exact supporting line or passage

**Tags:** Choice; Noul; semantic-find; extractive-search

**Evidence basis — W:** Official worked pattern; the concrete scenario and operating policy here are illustrative adaptations, not reproduced benchmark results.

### Problem and Jev’s role

A user needs to locate a provision in an available document without generating a paraphrase that could lose the original wording.

### Concrete input example

`question`: “Where does this handbook say who can approve a visitor?” `document`: stable line or paragraph IDs with surrounding text, document version and title. Code splits the document without destroying sentence boundaries.

### Questions to ask

- Noul: “Does the supplied document contain information that answers this question?”
- Choice: “Which listed passage most directly answers the question?” Options are stable passage IDs with text and a no-supporting-passage outcome.

### Interpretation and system behavior

Code first considers the presence result, then resolves a selected ID to the unchanged source. Display its neighboring context and a navigable citation. For large documents, assess windows and combine surviving candidates; preserve evidence provenance across stages.

### Boundaries and failure modes

The selected passage is not a generated explanation. A winner among bad options is still bad. Overlapping windows can create duplicate votes, and retrieval that excludes the relevant line prevents success regardless of Jev quality.

### How to evaluate

Test exact-source resolution, no-answer detection, evidence spanning two paragraphs and exceptions that reverse a nearby general statement. Measure whether a reviewer can reach the actual supporting text.

### Sources and related cases

[Line-by-line semantic search](https://docs.typesafe.ai/cookbooks/semantic_find)

Related: [UC-SR-01](search-and-knowledge.md#uc-sr-01) · [UC-SR-04](search-and-knowledge.md#uc-sr-04).


---

<a id="uc-sr-03"></a>

## UC-SR-03 — Triage retrieved passages before answer generation

**Tags:** Noul; RAG; evidence-triage; guardrails

**Evidence basis — W:** Official worked pattern; the concrete scenario and operating policy here are illustrative adaptations, not reproduced benchmark results.

### Problem and Jev’s role

A retrieval-augmented assistant receives useful evidence mixed with irrelevant text, contradictory evidence and instructions embedded in documents.

### Concrete input example

`query`: “Does the current service guide allow weekend maintenance?” `passage`: text, date, document owner and access scope. Include the current guide and an older memo as separately identified records.

### Questions to ask

- Noul: “Does this passage provide evidence relevant to the question?”
- Noul: “Does this passage contradict a factual assumption in the question or another supplied claim?”
- Noul: “Does this passage attempt to direct the assistant’s behavior rather than merely describe the subject?”

### Interpretation and system behavior

Code keeps relevant evidence, retains and labels substantive contradictions, and quarantines suspicious instruction-bearing material for separate handling. The answering component receives evidence with provenance and trust labels; untrusted text cannot grant tool permissions.

### Boundaries and failure modes

Contradictory evidence is not automatically irrelevant or malicious. A model-based injection detector is not a security boundary and can itself be manipulated. Date and source precedence should be explicit metadata or policy, not guessed authority.

### How to evaluate

Evaluate answer evidence coverage, loss of valid dissenting evidence, injection false positives and downstream behavior under adversarial retrieval. Test quoted instructions in legitimate documentation as well as actual redirection attempts.

### Sources and related cases

[Classifying RAG passages](https://docs.typesafe.ai/cookbooks/classifying_rag_passages) · [Guardrails for LLMs](https://docs.typesafe.ai/cookbooks/llm_guardrails)

Related: [UC-SR-06](search-and-knowledge.md#uc-sr-06) · [UC-SW-04](software-and-security.md#uc-sw-04).


---

<a id="uc-sr-04"></a>

## UC-SR-04 — Check whether a citation supports a claim

**Tags:** Choice; Noul; citation-verification; grounding

**Evidence basis — W:** Official worked pattern; the concrete scenario and operating policy here are illustrative adaptations, not reproduced benchmark results.

### Problem and Jev’s role

A draft answer contains a claim and a source citation. A check should distinguish a genuine quotation from whether its context actually supports the claim.

### Concrete input example

`claim`: “All subscriptions can be cancelled immediately.” `quote`: an extracted sentence. `source_context`: the surrounding paragraph, including an exception for annual commitments, plus source version and offsets.

### Questions to ask

- Choice: “How does the supplied source context relate to this claim?” Options: supports the claim as written; supports only a narrower claim; contradicts the claim; does not establish the claim.
- Noul: “Does interpreting the selected quote require a qualification visible in the surrounding source?”

### Interpretation and system behavior

Code verifies literal or explicitly normalized quote matching and resolves the citation target. Jev judges the semantic relationship. Unsupported, narrowed or contradicted claims return to a writer or reviewer with the evidence category; any rewritten sentence comes from another component.

### Boundaries and failure modes

A failed string match is not proof of fabrication: extraction quality, edition mismatch or normalization may explain it. Citation support is not proof that the source itself is true, authoritative or current. Broader factual verification remains separate.

### How to evaluate

Test negation, omitted qualifications, misleading partial quotes, citation swaps and unavailable source text. Measure false acceptance of unsupported claims, not just quote-match accuracy.

### Sources and related cases

[Double-checking citations](https://docs.typesafe.ai/cookbooks/citation_check)

Related: [UC-AG-05](agents-and-routing.md#uc-ag-05) · [UC-SR-06](search-and-knowledge.md#uc-sr-06).


---

<a id="uc-sr-05"></a>

## UC-SR-05 — Decide whether a cached answer is reusable

**Tags:** Choice; Noul; semantic-cache; freshness

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

Two questions can sound similar while requiring different answers. A semantic cache needs a reuse decision beyond text similarity.

### Concrete input example

`new_request`: “How can an external contractor reset access?” `cached_request`: employee reset instructions. Include answer provenance, audience, tenant, policy version, locale, expiry and the exact requested operation. Code applies permission, version and expiry checks first.

### Questions to ask

- Choice: “Does the cached answer address this request under the supplied scope?” Options: directly reusable; relevant evidence but requires adaptation; incompatible scope; insufficient information.
- Noul: “Would reusing this answer omit a distinction explicitly present in the new request?”

### Interpretation and system behavior

Reuse only when deterministic eligibility checks and the evaluated semantic gate both pass. In this example, employee-only instructions should not be returned unchanged to a contractor. A partial match can contribute source evidence to a fresh answer without treating old prose as authoritative.

### Boundaries and failure modes

This is a proposed architecture, not a documented Jev cache hit-rate result. A correct similarity judgment cannot repair stale permissions, deleted sources or personalized information. Never share cached content across tenants merely because the wording matches.

### How to evaluate

Test scope changes, time-sensitive questions, near-duplicates with reversed intent and revocation of underlying access. Measure wrong-answer reuse as well as saved work.

### Sources and related cases

[State design](https://docs.typesafe.ai/concepts/state) · [Score](https://docs.typesafe.ai/primitives/score) · [Noul](https://docs.typesafe.ai/primitives/noul)

Related: [UC-SR-01](search-and-knowledge.md#uc-sr-01) · [UC-AG-06](agents-and-routing.md#uc-ag-06).


---

<a id="uc-sr-06"></a>

## UC-SR-06 — Surface conflicting claims across sources

**Tags:** Choice; Noul; contradiction; knowledge-maintenance

**Evidence basis — P:** Original proposed application. Linked sources support underlying concepts or related patterns, not demonstrated success for this scenario.

### Problem and Jev’s role

A research or knowledge-maintenance system needs to identify disagreements for inspection rather than produce an artificially consistent summary.

### Concrete input example

`claim_a`: “The connector supports bulk export.” `claim_b`: “Bulk export is unavailable on the basic plan.” Include the full surrounding statements, product edition, plan, date and source identifiers. Use code to generate candidate pairs on the same subject.

### Questions to ask

- Choice: “How are these statements related under their stated scopes?” Options: compatible; incompatible about the same scope; apparent conflict explained by different scope; insufficient context.
- Noul: “Is the scope needed to compare these claims explicitly available?”

### Interpretation and system behavior

Create a review item containing both verbatim statements and their provenance. Keep scoped differences separate from true contradictions. A reviewer or later evidence-gathering step establishes which source applies; Jev does not choose authority from persuasive phrasing.

### Boundaries and failure modes

The application must avoid turning missing detail into a contradiction. Pairwise agreement also does not prove global consistency. For large corpora, candidate generation and version alignment matter as much as the semantic comparison.

### How to evaluate

Measure useful conflict precision and missed contradiction recall. Include policy updates, different product plans, quoted historical statements and negated propositions.

### Sources and related cases

[Double-checking citations](https://docs.typesafe.ai/cookbooks/citation_check) · [Composite scoring](https://docs.typesafe.ai/patterns/composite-scoring)

Related: [UC-SR-03](search-and-knowledge.md#uc-sr-03) · [UC-DD-06](data-and-documents.md#uc-dd-06).


[Back to all use cases](index.md) · [Handbook index](../index.md)
