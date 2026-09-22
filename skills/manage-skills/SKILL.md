---
name: manage-skills
description: Manages the user's skill collection in the agent-skills repo. Lists which skills Claude Code or Codex sees, adds vendor skills, writes new own skills, adopts a vendor skill through an interview, retires skills, edits the manifest, and runs the weekly or monthly review. Use when the user asks what skills a harness sees, wants to add, adopt, retire or review skills, or wants the manifest changed.
---

# Manage skills

The collection lives in `/home/wkuczkowski/projects/TOOLS/skills` (below: the repo). Run every `bin/*` and `npx skills` command with the repo as the working directory, whatever directory the session started in. The repo's `AGENTS.md` holds the rules and `CONTEXT.md` the vocabulary; read them once per session before changing anything. The user's instructions take precedence over this skill.

## Conventions

- `manifest.yaml` is the source of truth for what exists and which harness sees it; `bin/link` projects it onto `~/.claude/skills` and `~/.agents/skills`. After any change to the manifest, `skills/`, `private/` or `.agents/skills/`, run `bin/link` and then `bin/link check`. A clean check reports 0 findings; `~/.claude/skills/synced` (the desktop app's account skills) is exempt.
- Vendor skills under `.agents/skills/` stay verbatim; `npx skills` owns them. Changing one means adopting it.
- Own skills default to both harnesses and model invocation. A user-only own skill needs both `disable-model-invocation: true` in the frontmatter and `agents/openai.yaml` with `policy.allow_implicit_invocation: false`; the frontmatter is the truth and `bin/link check` reports mismatches.
- Retiring a skill, deleting files and changing skill content happen only after the user has said so. Reviews propose; they do not apply.
- English throughout, in skills and in the report. Reports the user reads are HTML on the house template; Markdown stays for agent-facing files.

## Operations

**list.** `bin/link list claude-code` or `bin/link list codex` prints name, kind, invocation mode, status and source path. Answer from that output alone; compare harnesses only after running both lists. A Codex session started inside the repo additionally sees every vendor skill through the project `.agents/skills` root, whatever the manifest says.

**status.** `bin/link check` plus a summary of the manifest: counts per kind, drafts, `keep: true`, skills narrowed to one harness, adopted skills and their upstream. Manifest edits the user may ask for here: narrow a skill with `harnesses: [claude-code]` or `[codex]`; mark `status: draft` or `keep: true`; move an own skill between `skills/` (public, pushed to GitHub) and `private/` (this machine only) with `git mv`, the manifest entry stays as it is. Then `bin/link` and `bin/link check`.

**add-vendor.** In the repo: `npx skills add <owner/repo> -a codex -y -s <name>`. Add a manifest entry with `kind: vendor` and `upstream: <source> <skillPath>` copied from the new `skills-lock.json` entry, plus `harnesses:` when the user wants one harness only. `bin/link`, `bin/link check`.

**new.** Write the skill under `skills/<name>/` in the register of `global/AGENTS.md` (facts and observations, imperatives only where a wrong guess costs something real) and with the mechanics in the `writing-for-agents` skill: name in lowercase letters, digits and hyphens matching the directory; a third-person description that states what the skill does and when it applies; short body with substantial reference in `references/`. Add `agents/openai.yaml` with an `interface` block (`display_name`, `short_description` of 25 to 64 characters, `default_prompt` mentioning `$<name>`). Manifest entry `kind: own`. `bin/link`, `bin/link check`; the check runs `claude plugin validate` over `skills/`.

**adopt.** Turn a vendor skill into an own skill through the interview in [references/adopt.md](references/adopt.md). The result is a rewrite under `skills/<same name>`, a manifest entry with `adopted: true` and a pinned upstream, and the vendor copy removed from `skills-lock.json`.

**retire.** Confirm the skill and the consequence with the user first. Vendor: `bin/unvendor <name>` in the repo (never `npx skills remove`, it also deletes an own directory of the same name). Own: delete `skills/<name>` or `private/<name>` (`git rm -r` when tracked). Remove the manifest entry. `bin/link`, `bin/link check`.

**review weekly** and **review monthly.** Follow [references/review.md](references/review.md). Weekly writes `usage.yaml`, runs `workflow-from-chats` for the candidates from conversations, reads own skills and both instruction files together for contradictions, and builds the report `reports/<date>/review.html` on the house template (`assets/report/README.md`) with the raw data files beside it; monthly adds a research refresh from [references/research-prompts.md](references/research-prompts.md), a compliance pass over every own skill and `global/AGENTS.md`, and the consistency read widened to vendor skills. `bin/review-run` is what the Monday timer calls; it picks weekly or monthly and runs the review headlessly.
