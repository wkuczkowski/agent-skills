# Use-case card template

[Handbook index](../index.md) · [Use-case index](../use-cases/index.md) · [Source registry](../sources.md)

Use a stable `UC-AREA-NN` identifier and an explicit matching HTML anchor. This template defines what a complete card contains; it is not an unimplemented case counted in the catalog.

## Required sections

| Section | Content required |
|---|---|
| Title and tags | Describe the application outcome; include primitive and architecture keywords. |
| Evidence basis | W: official worked pattern; E: external implementation/demo; P: proposed application. State exactly which aspect the source supports. |
| Problem and Jev’s role | Identify the semantic decision and explain why the task is not simply generation or exact code. |
| Concrete input example | A clearly hypothetical scenario, named state fields, evidence scope and candidate source. |
| Questions | Complete natural-language specifications, primitive, options or ordered rubric, and relevant no-match outcomes. |
| Result interpretation and system behavior | Explain what different judgments mean, which code branch consumes them and what observes the final outcome. |
| Boundaries and failure modes | Missing evidence, unsafe inferences, side-effect controls and why a correct type is not enough. |
| Evaluation | Independent target, important error, realistic difficult cases and at least one meaningful metric. |
| Sources and related cases | Primary-source links, access limitations, and relative links to complementary cards. |

## Evidence wording

For **W**, say that an official example demonstrates the underlying pattern; do not imply that your adapted scenario was benchmarked.

For **E**, identify the inspected project or author’s experiment and what was actually read or executed. A README review is not a source-code audit or a successful deployment. A scripted demonstration is not a live measurement.

For **P**, label the scenario as an original design proposal. Cite sources for the underlying primitives or patterns, not as proof that this application already works. A provider’s idea map alone does not establish an implemented use case.

## Writing rules for agent consumption

Keep the card independently interpretable, but link shared probability semantics and operational controls instead of copying them into every example. Mark questions as conceptual rather than API payloads. Specify new-state dependencies instead of implying that questions in one request can see each other’s answers.

Avoid fixed model IDs, package commands, prices, request limits, default thresholds and generalized speed claims. Link the relevant current contract. Use fictional input data and never invent observed model results, benchmark values, customers or production deployments.
