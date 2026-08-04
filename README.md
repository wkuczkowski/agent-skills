# agent-skills

Personal agent skills, installable with [`npx skills`](https://github.com/vercel-labs/skills):

```bash
npx skills add wkuczkowski/agent-skills -g -a '*'
```

## Skills

### Agent CLIs

- **claude-headless** — run Claude Code programmatically via the headless CLI (`claude -p`): output parsing, session resume, permissions, structured output, verified gotchas.
- **codex-headless** — run OpenAI Codex CLI programmatically via `codex exec`: gpt-\*-sol model + reasoning-effort selection, workspace-write sandbox, "Approve for me" auto-review, JSONL/schema output, session resume.
- **cursor-headless** — run Cursor Agent CLI programmatically via `cursor-agent -p`.

### Design

- **conversion-ux** — design paywalls, pricing, checkout, product pages and signup flows: evidence-graded behavioral levers, Baymard/NN-g findings, and the legal bright lines on deceptive patterns.
- **ui-patterns** — pick the right pattern for search, empty states, browse screens, numeric input, disclosure, personalization and post-purchase.
- **ux-critique** — audit an existing screen for usability, conversion and dark-pattern defects: two-stage find-then-filter, evidence contract per finding, hard exclusions.

## Provenance

The three design skills are built on a source-verification pass over the behavioral-science and UX claims they encode: every claim was checked against primary sources, and each verdict was then attacked by an adversarial second pass.

It matters because a lot of this field is folklore. Ten widely-repeated claims turned out to be **fabricated** (no primary source at any point in the citation chain), including "70–90% of users never change defaults", "free samples lift purchases 2,000%", and "transparency bias" — which is not a research construct at all. Several others are real effects with the wrong attribution, the wrong mechanism, or a contested effect size.

Rules in these skills are tagged `(measured)`, `(mechanism)`, `(untested)` or `(legal)`, and the tag governs how strongly they may be stated. `references/evidence.md` in `conversion-ux` carries the drop-list and the sample-size arithmetic that decides whether "just A/B test it" is advice or noise.

The legal sections are design guidance derived from primary sources, not legal advice — and the EU instruments were read from mirrors rather than the Official Journal, so confirm article-level wording before any of it drives a shipped compliance decision.
