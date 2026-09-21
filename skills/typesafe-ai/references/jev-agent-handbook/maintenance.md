# Maintenance and source freshness

[Handbook index](index.md) · Research reviewed: 2026-09-21

The package is designed to minimize avoidable staleness, not to remain correct without review. Local files contain design reasoning and illustrative workflows. Operational contracts remain linked to their owners.

## Refresh on the relevant change, not by rereading everything

| Trigger | Refresh |
|---|---|
| A new implementation | [Live documentation index](https://docs.typesafe.ai/llms.txt), selected primitive, current model and chosen SDK or gateway contract. |
| A model change | Model availability, version-specific limitations, representative evaluation and thresholds. |
| An SDK or wrapper upgrade | Installed types, changelog, request/response mappings, retries and contract tests. |
| A new sensitive-data category or hosting route | Provider and gateway agreements, processing location, retention and logging controls. |
| A taxonomy, rubric or policy change | Questions, candidate definitions, stored judgment compatibility and evaluation labels. |
| A new example or claimed breakthrough | Original source, publication date, actual implementation status and experimental conditions. |
| A moved or inaccessible source | Rediscover through the owning index or repository; mark the gap rather than guessing a replacement. |

The exact review schedule is an operational choice. Recheck a volatile fact before using it for a deployment decision, regardless of the date printed here.

## Follow current source authority

For native API behavior, prefer the current TypeSafe reference and the installed SDK’s types. For wrapper behavior, prefer the wrapper’s current documentation and installed package. A blog or old cookbook may use a historical contract. Resolve disagreements explicitly rather than combining fields from incompatible versions.

Start source discovery at [Live documentation index](https://docs.typesafe.ai/llms.txt). Try the documented Markdown representation when useful; if it fails, use the ordinary page. If neither is available, use verified installed types or known local documentation and disclose the limitation. Do not fabricate a successful read.

## Keep case IDs stable

Retain IDs such as `UC-DD-01` when editing descriptions. Add new IDs instead of renumbering existing ones. Preserve explicit anchor targets when moving files, and update both the use-case index and the official-cookbook coverage map. Keep related-case links reciprocal where that improves navigation, without requiring every relationship to be symmetric.

## Record what was actually checked

The [source registry](sources.md) distinguishes content read, index-listed references and unavailable bodies. A source’s presence does not imply that a project was run, a benchmark reproduced or a repository security-audited. Record new research dates per changed entry rather than silently changing the whole package’s review date.

For reproducible implementation research, save the exact model and dependency versions plus relevant repository commit IDs when obtainable. A live link is the route to current truth; a pinned implementation record is evidence of what was tested. Both are useful, but they serve different purposes.

## Package-level checks

After edits, validate local file paths, fragment anchors, unique case IDs and index coverage. Check that each case still distinguishes evidence from hypothetical design, identifies an uncertainty path and gives a meaningful evaluation target. Do not introduce fixed prices, universal thresholds or performance promises without dated evidence and scope.

Initial research date: **2026-09-21**. No automated refresh service is configured or implied by this package.
