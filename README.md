# agent-skills

Personal agent skills, installable with [`npx skills`](https://github.com/vercel-labs/skills):

```bash
npx skills add wkuczkowski/agent-skills -g -a '*'
```

## Skills

### Managing skills and instructions

- **manage-skills** — lists what Claude Code or Codex sees, adds vendor skills, adopts a vendor skill through an interview, retires skills, and runs a weekly or monthly review of the whole collection (usage from transcripts, upstream changes, drift, vendor-guidance refresh).
- **writing-for-agents** — the writing manual for skills, `AGENTS.md`/`CLAUDE.md` and pointer-reached docs, with harness mechanics for Claude Code and Codex and per-model guidance for Claude Fable 5.1 and GPT-6 Astra. Adopted from [mattpocock/skills](https://github.com/mattpocock/skills).

- **orchestrate** — user-invoked: run the main agent as an orchestrator that protects its own context and delegates reading, building, testing, research and the review of delivered work to subagents.
- **unslop** — user-invoked: cut AI tells from writing. Copy of [cursor/plugins](https://github.com/cursor/plugins) pstack/skills/unslop, kept under evaluation.

### Machine hygiene

- **vetting-dependencies** — before any package, image, action or installer is pulled from the network: decide whether it is worth adding at all, have a subagent check owner, releases, advisories and typosquatting, state a verdict.
- **exposing-services** — expose a dev server or container to the LAN behind ufw and a temporary firewall rule, diagnose a service that does not answer, close the port when done.

### Workflows

- **research** — investigate a question against primary sources, delegating the reading to one or several subagents, and write verified findings into the repo for other agents to use. Adopted from [mattpocock/skills](https://github.com/mattpocock/skills).
- **workflow-from-chats** — mine Codex and Claude Code conversations since the last run for proposals (new skills, skill edits, instruction diffs) in the form `manage-skills new` takes, deduplicated against the manifest, with links to the transcript turns behind each, as an HTML report on the house template.

### Agent CLIs

- **claude-headless** — run Claude Code programmatically via the headless CLI (`claude -p`): output parsing, session resume, permissions, structured output, verified gotchas.
- **codex-headless** — run OpenAI Codex CLI programmatically via `codex exec`: gpt-\*-sol model + reasoning-effort selection, workspace-write sandbox, "Approve for me" auto-review, JSONL/schema output, session resume.
- **cursor-headless** — run Cursor Agent CLI programmatically via `cursor-agent -p`, in normal or Fast mode depending on the task.

### Design

- **conversion-ux** — user-invoked: design paywalls, pricing, checkout, product pages and signup flows: evidence-graded behavioral levers, Baymard/NN-g findings, and the legal bright lines on deceptive patterns.
- **ui-patterns** — user-invoked: pick the right pattern for search, empty states, browse screens, numeric input, disclosure, personalization and post-purchase.
- **ux-critique** — user-invoked: audit an existing screen for usability, conversion and dark-pattern defects: two-stage find-then-filter, evidence contract per finding, hard exclusions.

## Provenance

The three design skills are built on a source-verification pass over the behavioral-science and UX claims they encode: every claim was checked against primary sources, and each verdict was then attacked by an adversarial second pass.

It matters because a lot of this field is folklore. Ten widely-repeated claims turned out to be **fabricated** (no primary source at any point in the citation chain), including "70–90% of users never change defaults", "free samples lift purchases 2,000%", and "transparency bias" — which is not a research construct at all. Several others are real effects with the wrong attribution, the wrong mechanism, or a contested effect size.

Rules in these skills are tagged `(measured)`, `(mechanism)`, `(untested)` or `(legal)`, and the tag governs how strongly they may be stated. `references/evidence.md` in `conversion-ux` carries the drop-list and the sample-size arithmetic that decides whether "just A/B test it" is advice or noise.

The legal sections are design guidance derived from primary sources, not legal advice — and the EU instruments were read from mirrors rather than the Official Journal, so confirm article-level wording before any of it drives a shipped compliance decision.
