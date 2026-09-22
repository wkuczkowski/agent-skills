# Adopting a vendor skill

Adoption turns `.agents/skills/<name>` (vendor, verbatim, tracked by `npx skills`) into `skills/<name>` (own, rewritten for the user, pinned to the upstream commit it started from). The name stays. The text does not: the own skill is written from the interview, in the repo's voice, using what the vendor skill got right. Never a verbatim copy.

## Before the interview

1. Read the vendor skill completely: `SKILL.md`, every file under `references/`, `scripts/`, `assets/`, and `agents/openai.yaml` if present.
2. Read its usage: `usage.yaml` at the repo root (both harnesses, count, last invocation, sessions). Run `bin/usage --write` first when the file is older than a week.
3. Run `bin/upstream --diff <name>`. `unchanged` means the installed files match upstream HEAD. `changed` means upstream moved since the install; show the user the diff before the interview so the rewrite starts from a known version.
4. Note the invocation modes the vendor skill declares: `disable-model-invocation` in the frontmatter, `policy.allow_implicit_invocation` in `agents/openai.yaml`. They are the default answer for round 3.

## The interview

Ask in three rounds. Every question carries a recommended answer, derived from the reading above, so the user can accept with one word or correct it. Ask the next round only after the previous one is answered; a round's answers change the next round's recommendations.

**Round 1, role.** What the skill does in the user's workflow: the situations it fires in, what it produces, what would be missed if it were gone. Recommendation: the description's trigger cases, checked against usage (a skill invoked 30 times from Codex and never from Claude Code has a different role than the description claims). Ask which of the skill's branches the user actually reaches.

**Round 2, cut and add.** Walk through the skill section by section and propose, for each, keep, cut or rewrite, with a one-line reason: sections that repeat what the models already do by default, branches the user never reaches, instructions written for older models (the prescriptive "you MUST" register, enumerated behaviours a brief instruction now covers), references to tools or trackers the user does not have. Then propose what to add: the repo's conventions the skill should know (issue tracker under `.scratch/`, triage labels, `CONTEXT.md` and ADRs, `uv` and `pnpm`, the headless skills for delegation), the user's own habits observed in usage, and anything from the current research notes (`research/*-<date>.md`, newest of each topic) that changes how the skill should be written for Fable 5.1 and Astra.

**Round 3, invocation and harnesses.** Model-invocable or user-only, per harness. Recommendation: keep the vendor's mode unless usage shows the opposite (a user-only skill the user types every day may deserve model invocation; a model-invocable skill that never fires on its own is context load for nothing). Harnesses: both, unless the skill depends on one harness's tool. Confirm the name stays.

Close the interview with a short summary of the decisions and wait for the user's go-ahead before writing.

## Writing the own skill

1. Write `skills/<name>/SKILL.md` in the register of `global/AGENTS.md` and with the mechanics in the `writing-for-agents` skill: description in third person with the trigger cases the user confirmed, short body, substantial reference under `references/`. Reuse the vendor skill's ideas; write the sentences yourself. Keep `scripts/` and `assets/` the user asked to keep, copied as files.
2. Invocation: model-invocable needs nothing. User-only needs `disable-model-invocation: true` in the frontmatter and `agents/openai.yaml` with `policy.allow_implicit_invocation: false`. Add an `interface` block (`display_name`, `short_description`, `default_prompt` mentioning `$<name>`) in either case.
3. Find the commit to pin. From `skills-lock.json` take `source` (owner/repo) and `skillPath`; the skill directory is the parent of `skillPath`. Then:

   ```bash
   gh api "repos/<owner/repo>/commits?path=<dir>&per_page=1" --jq '.[0].sha'
   ```

   That is the last commit touching the directory on the default branch, which is the version installed when `bin/upstream` said `unchanged`. When it said `changed`, the pinned commit is still this one, and the rewrite is based on the upstream HEAD content the user saw in the diff.
4. Manifest entry, replacing the vendor entry under the same name:

   ```yaml
   <name>:
     kind: own
     adopted: true
     upstream: <owner/repo> <skillPath>@<sha>
   ```

   The `upstream` line is exactly `owner/repo path/to/SKILL.md@<commit>`; `bin/upstream` parses this form and reports `unknown` for anything else. Add `harnesses:` only when the user narrowed them.
5. In the repo: `bin/unvendor <name>`. This deletes `.agents/skills/<name>` and the lock entry and nothing else. `npx skills remove` is off limits here: it also deletes `skills/<name>` when that directory exists.
6. `bin/link`, then `bin/link check`. Expect 0 findings. Confirm `bin/upstream` lists the skill as `own`, `unchanged`.
7. Tell the user what changed: files written, manifest diff, lock entry removed, projections relinked.
