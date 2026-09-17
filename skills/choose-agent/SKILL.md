---
name: choose-agent
description: Assigns work to a model by the harness the main agent runs in, choosing between Opus 5, Fable 5.1 and Astra with the effort to run at. Use when picking the model for a subagent, a workflow or a headless run of the other harness.
disable-model-invocation: true
---

# Choose agent

Which model does which work depends on the harness the main agent runs in. Identify it from the current session and exposed tools, not from installed CLI binaries. Read the table for that harness. The user's explicit choice overrides the table. When unsure between two rows, pick the cheaper option and say why. Cheap and fast lookups (a fact, a package check, a version) go to Astra at low effort in either harness.

Model ids: `claude-fable-5-1`, `claude-opus-5`, `gpt-6-astra`.

## Execution route

Always use the current harness's native mechanism for a model it supports. Work directly when the current agent is the selected model; when delegating, use the native subagent or workflow mechanism exposed in this session and its supported model and effort parameters.

- In Codex, run Astra natively; never launch `codex-headless` or `codex exec` to obtain Astra.
- In Claude Code, run Fable and Opus natively through the Agent tool (`model: fable|opus`) or a native workflow; never launch `claude-headless` or `claude -p` to obtain them.
- Use headless for a model outside the current harness: normally `claude-headless` for Fable or Opus from Codex, and `codex-headless` for Astra from Claude Code. Read the corresponding skill for invocation details.

The only exception to the native rule is a deliberate test of the headless CLI, skill loading or integration itself. State what the headless run is testing. An ordinary task that includes writing or running tests is not this exception. If the required native mechanism is unavailable, report that limitation instead of silently falling back to headless for the same harness.

## Main agent in Claude Code

| Work | Model, effort |
|---|---|
| Workflows and default subagents | Opus 5, high |
| Architecture, design, creative thinking, the bigger picture, debugging the hardest problems | Fable 5.1, high |
| Code review, bugs, edge cases | either: Fable 5.1 high when the problem is hard or the design matters, Astra for the ordinary case |
| Implementation in general | Astra, effort per `codex-headless` |
| Second opinion | Astra, high |
| Cheap and fast lookups | Astra, low effort, via `codex-headless` |

## Main agent in Codex

| Work | Model, effort |
|---|---|
| Architecture, everything design-related, the heavier planning | Fable 5.1, high, via `claude-headless` |
| The hardest bugs, code review, edge cases | Astra and Fable 5.1 together, see below |
| Everything else | Astra, effort per the task |
| Cheap and fast lookups | Astra, low effort |

Astra and Fable together, one proposes and the other challenges: Astra writes its diagnosis or review as claim, evidence and proposed fix to a file in a private run directory; one `claude-headless` run at Fable high gets the same files plus that file and the task of finding what is wrong or missing, answering with agreed and disputed points and the evidence for each; Astra answers the disputes, and a second Fable run follows only if disputes remain. Converged means both sides agree on every point; after two rounds report the remaining split to the user with both positions.
