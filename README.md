# agent-skills

Personal agent skills, installable with [`npx skills`](https://github.com/vercel-labs/skills):

```bash
npx skills add wkuczkowski/agent-skills -g -a '*'
```

## Skills

### Managing skills and instructions

- **manage-skills** — lists what Claude Code, Codex or Antigravity sees, adds vendor skills, adopts a vendor skill, retires skills, and runs a weekly or monthly review of the whole collection (usage from transcripts, upstream changes, drift, vendor-guidance refresh).
- **writing-for-agents** — what earns a line in a skill or instruction file (facts, dated lessons, what the user rejected, the goal rather than the steps) and the mechanics of a finished one: layout, frontmatter, invocation flags, what Claude Code, Codex and Antigravity load, how a skill is proven to fire.

### Engineering and planning

- **codebase-design** — deep modules, seams, interface-level tests and designing it twice when planning a new subsystem, refactoring or rethinking code architecture. Adopted from [mattpocock/skills](https://github.com/mattpocock/skills).
- **council** — independent cross-model critique (one critic from another model family by default; full three-model council of Claude Fable 5.2 High, GPT-6.2 Sol High and Gemini 4 Neon High on major decisions or when requested). Adopted from [t3dotgg/tstack](https://github.com/t3dotgg/tstack).
- **grilling** — user-invoked: stress-test a plan, putting to the user only the decisions that are his (preference, scope, cost, direction) and settling technical ones itself, listed so he can override. Adopted from [mattpocock/skills](https://github.com/mattpocock/skills).
- **grill-with-docs** — user-invoked: `grilling` plus a glossary and ADRs written as decisions settle, through `domain-modeling`; works across harnesses. Adopted from [mattpocock/skills](https://github.com/mattpocock/skills).
- **domain-modeling** — keeps a project's `GLOSSARY.md` in the user's own meaning of his terms (an agent can read what the code calls things; what he means is what a new session cannot guess) and records hard-to-reverse decisions as ADRs. Adopted from [mattpocock/skills](https://github.com/mattpocock/skills).
- **unslop** — user-invoked: cut AI tells from writing. Copy of [cursor/plugins](https://github.com/cursor/plugins) pstack/skills/unslop.

### Machine hygiene

- **vetting-dependencies** — before any package, image, action or installer is pulled from the network: decide whether it is worth adding at all, have a subagent check owner, releases, advisories and typosquatting, state a verdict.
- **exposing-services** — expose a dev server or container to the LAN behind ufw and a temporary firewall rule, diagnose a service that does not answer, close the port when done.

### Workflows

- **research** — investigate a question against primary sources, delegating the reading to one or several subagents, and write verified findings into the repo for other agents to use. Adopted from [mattpocock/skills](https://github.com/mattpocock/skills).
- **workflow-from-chats** — user-invoked: mine Codex and Claude Code conversations since the last run for proposals (new skills, skill edits, instruction diffs), deduplicated against the manifest, answered in the chat, with the evidence and ready diffs in a working file for the agent that applies them.

### Agent CLIs

- **antigravity-headless** — run Google Antigravity CLI programmatically via `agy -p`: model selection, stream-json output, allow-rule permissions, verified gotchas.
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
