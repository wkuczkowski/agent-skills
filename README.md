# agent-skills

Personal agent skills, installable with [`npx skills`](https://github.com/vercel-labs/skills):

```bash
npx skills add wkuczkowski/agent-skills -g -a '*'
```

## Skills

### Managing skills and instructions

- **manage-skills** — lists what Claude Code or Codex sees, adds vendor skills, adopts a vendor skill through an interview, retires skills, and runs a weekly or monthly review of the whole collection (usage from transcripts, upstream changes, drift, vendor-guidance refresh).
- **writing-for-agents** — the mechanics of a finished skill or instruction file: layout, frontmatter, invocation flags, what Claude Code and Codex load, how a skill is proven to fire. The register itself lives in the global instructions.

- **diagnosing-bugs** — holds a diagnosis at reproducing and locating the failure before code changes. Adopted from [mattpocock/skills](https://github.com/mattpocock/skills) and thinned to that one correction.
- **codebase-design** — deep modules, seams, testing through the interface: the vocabulary other skills use and the user's design preferences. Adopted from mattpocock/skills.
- **orchestrate** — user-invoked: run the main agent as an orchestrator that protects its own context and delegates reading, building, testing, research and the review of delivered work to subagents.
- **pausing-work** — user-invoked: bring running work to a safe stop within the time given, stop background tasks, and leave a handoff file (and a new-session prompt when asked).
- **grilling** — user-invoked: stress-test a plan, putting to the user only the decisions that are his (preference, scope, cost, direction) and settling technical ones itself, listed so he can override. Adopted from [mattpocock/skills](https://github.com/mattpocock/skills).
- **grill-with-docs** — user-invoked: `grilling` plus a glossary and ADRs written as decisions settle, through the vendor `domain-modeling` skill; works in Codex too. Adopted from mattpocock/skills.
- **unslop** — user-invoked: cut AI tells from writing. Copy of [cursor/plugins](https://github.com/cursor/plugins) pstack/skills/unslop, kept under evaluation.

### Machine hygiene

- **vetting-dependencies** — before any package, image, action or installer is pulled from the network: decide whether it is worth adding at all, have a subagent check owner, releases, advisories and typosquatting, state a verdict.
- **exposing-services** — expose a dev server or container to the LAN behind ufw and a temporary firewall rule, diagnose a service that does not answer, close the port when done.

### Workflows

- **research** — investigate a question against primary sources, delegating the reading to one or several subagents, and write verified findings into the repo for other agents to use. Adopted from [mattpocock/skills](https://github.com/mattpocock/skills).
- **workflow-from-chats** — mine Codex and Claude Code conversations since the last run for proposals (new skills, skill edits, instruction diffs) in the form `manage-skills new` takes, deduplicated against the manifest, answered in the chat, with the evidence and ready diffs in a working file for the agent that applies them.

### Agent CLIs

- **claude-headless** — run Claude Code programmatically via the headless CLI (`claude -p`): output parsing, session resume, permissions, structured output, verified gotchas.
- **codex-headless** — run OpenAI Codex CLI programmatically via `codex exec`: model and reasoning effort passed explicitly, workspace-write sandbox, "Approve for me" auto-review, JSONL/schema output, session resume.
- **cursor-headless** — run Cursor Agent CLI programmatically via `cursor-agent -p`, in normal or Fast mode depending on the task.

### Design

- **ux** — user-invoked: design or audit pricing, checkout, paywalls, product pages, signup, landing pages, search, forms, empty states and internal app screens; a routing table picks the references per screen: evidence-graded behavioral levers, Baymard/NN-g findings, the legal bright lines on deceptive patterns, and a two-stage find-then-filter audit with an evidence contract per finding.

## Provenance

The `ux` skill is built on a source-verification pass over the behavioral-science and UX claims it encodes: every claim was checked against primary sources, and each verdict was then attacked by an adversarial second pass.

It matters because a lot of this field is folklore. Ten widely-repeated claims turned out to be **fabricated** (no primary source at any point in the citation chain), including "70–90% of users never change defaults", "free samples lift purchases 2,000%", and "transparency bias" — which is not a research construct at all. Several others are real effects with the wrong attribution, the wrong mechanism, or a contested effect size.

Rules in `ux` are tagged `(measured)`, `(qualitative)`, `(mechanism)`, `(untested)` or `(legal)`, and the tag governs how strongly they may be stated. `references/evidence.md` in `ux` carries the drop-list and the sample-size arithmetic that decides whether "just A/B test it" is advice or noise.

The legal sections are design guidance derived from primary sources, not legal advice — and the EU instruments were read from mirrors rather than the Official Journal, so confirm article-level wording before any of it drives a shipped compliance decision.
