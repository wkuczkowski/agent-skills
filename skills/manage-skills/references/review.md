# Review

Weekly: usage, unused candidates, candidates from conversations, upstream changes, drift, consistency, proposals. Monthly: the same plus a research refresh, a compliance pass, and consistency widened to vendor skills. Both write one report for the user, `reports/<date>/review.html` with `<date>` from `date +%F`, on the house template (`assets/report/README.md`), and keep the raw command outputs next to it so every number in the report can be traced. The report proposes; the only files written are `usage.yaml` (by `bin/usage --write`), the report directory, `reports/.workflow-from-chats-last-run` and, monthly, the new research files.

Every section below appears in every report, in this order, even when its content is `none` followed by the command that said so. A section is complete when it is written from this run's command output; a section written from memory or from a previous report is not done. When the user says the data files for today already exist, steps 1 to 3 are read from those files instead of re-run.

## Weekly steps

Run from the repo. `D=reports/$(date +%F)`; `mkdir -p "$D"`.

1. `bin/usage --write > "$D/usage.txt"`, then `bin/usage --unused > "$D/unused.txt"`.
2. `bin/upstream --json > "$D/upstream.json"` (exit 1 means at least one skill changed; that is a result, not an error). For each record with `status: changed`: `bin/upstream --diff <name> > "$D/upstream-<name>.diff"` and read the diff.
3. `bin/link check > "$D/check.txt"` (exit 1 whenever there are findings; the `unslop` finding is the known baseline).
4. Candidates from conversations: when `"$D/conversations.html"` does not exist yet, read `skills/workflow-from-chats/SKILL.md` and carry out that skill with its default window (since its last run). It is user-only, so it is followed by reading the file, not through the Skill tool. Its report lands at `"$D/conversations.html"`; when the file already exists from today, use it as it is.
5. Consistency: read together everything an agent loads together and note each contradiction in `"$D/consistency.txt"` (file and line for both sides, the kind, the proposed resolution). Weekly scope: every own skill under `skills/` and `private/`, `global/AGENTS.md` and the repo's `AGENTS.md`, since these change often. Monthly scope: also every vendor skill a harness sees (`bin/link list claude-code`, `bin/link list codex`). A contradiction is one of four kinds: two skills claiming the same trigger (compare descriptions first, then bodies); a skill contradicting a rule in the global instructions; one rule stated twice with different wording; a reference to a path, flag or command that no longer exists (confirm with `ls`, `--help` or the harness). Done when every file in scope has been read in this run and each finding carries both quotes.
6. Write the sections fragment `"$D/review-sections.html"` as specified below, then build:

   ```bash
   assets/report/build "$D/review-sections.html" --out "$D/review.html" \
     --title "Skill review $(date +%F)" --eyebrow "manage-skills · weekly" \
     --subtitle "<one sentence: candidates, upstream changes, drift>" \
     --chip "Kind=weekly" --chip "Usage window=12 weeks" --chip "usage.yaml=<generated time>" \
     --foot "Written by the manage-skills review from usage.txt, unused.txt, upstream.json, check.txt, consistency.txt and conversations.html in this directory."
   ```

   Done when the build exits 0.

## Report sections

Section ids in order: `summary`, `usage`, `unused`, `conversations`, `upstream`, `drift`, `consistency`, `proposals`; monthly adds `research` and `compliance` before `proposals`. Text from files and diffs goes through `data-src` or is escaped.

**summary.** A `brief` ledger: kind (weekly or monthly), usage window, `usage.yaml` generation time, unused candidates (count), conversation proposals (count per grade), upstream changes (count), drift findings (new ones, apart from `unslop`), consistency findings (count), and the list of files this run wrote.

**usage.** A `tbl` with the columns of `usage.txt`: skill, harness, invocations, last invoked, sessions; one row per skill and harness, all of them, zero rows included, numbers in `class="num"` cells. Below it a second table for the `not in manifest` block, when present, with its `class` column (`claude-code built-in`, `codex system`, `project skill`, `retired`, `unknown`) and `where`, followed by the one-line note from `usage.txt` that built-ins and system skills stay live in their harness whatever the repo says.

**unused.** The rows of `unused.txt` as a table: skill, kind, mode, threshold, last invocation (the date, or `none`; drop the word `last`). State the rule once: six weeks without invocation for a model-invocable skill, twelve for a user-only skill, `keep: true` exempt. Then the Codex caveat in a `callout--warn`: `bin/usage` counts a Codex invocation whenever a tool call reads `<skill root>/<name>/SKILL.md`, so opening a skill to edit it, or working on this repo with Codex, counts as use. The counts cannot tell the two apart, so name the skills whose only Codex invocations fall on a day with work on this repo as possibly unused, without calling them candidates. Retirement is proposed here and decided by the user.

**conversations.** From `"$D/conversations.html"`: the window it covered, the counts per grade, and one line per strong or medium proposal (id, title, kind, grade pill) linking to `conversations.html#<id>` with `file://` and the absolute path; then the decisions it left open. When the workflow run failed or found nothing, say which and link the report anyway.

**upstream.** One entry per skill in `upstream.json` whose status is `changed`: skill, upstream, files modified, added, removed, a summary of the diff in two or three sentences that says what changed in substance (a new section, a reworded rule, a fixed command) and whether it touches the branches the user reaches, and the diff itself folded in `details` with `<pre class="diff" data-src="upstream-<name>.diff">`. A vendor skill whose `local_drift` is true has been edited in place; report that as a finding. Then a list of `unknown` records with their reason, and the one-line repo summary from `repos`. When nothing changed: `none` and the `unchanged` count.

**drift.** `<pre class="code" data-src="check.txt">`, then a sentence separating the known finding (`unslop`) from anything new.

**consistency.** The scope this run read (own skills and both instruction files weekly; vendor skills as well monthly), then one entry per finding from `consistency.txt`: the kind (same trigger, against a global rule, restated rule, dead reference) and a two-column `tbl` with the two conflicting quotes side by side, each headed by its file and line, followed by a proposed resolution in a sentence or two: which wording wins and where the other goes, or which reference to fix. A vendor skill is not edited; there the resolution is a manifest narrowing, an adoption or a line in the own skill or instruction file. When nothing conflicts: `none` and the count of files read.

**proposals.** Ready diffs, nothing applied. Retirement candidates: a unified diff removing the manifest entry in `<pre class="diff">`, plus the command that removes the files (`bin/unvendor <name>` for vendor, `git rm -r skills/<name>` for own). Vendor upstream changes: `npx skills update <name>` as the command, with a note that a vendor skill with `local_drift` loses its edits. Adopted skills whose upstream changed: the specific edits worth carrying over, as a diff against `skills/<name>`. Drift: the manifest or file change that clears each new finding. Consistency: the edit that settles each finding, as a diff against the own skill or instruction file. Conversation proposals are not repeated here; the `conversations` section links them. When there is nothing to propose, say so.

## Monthly additions

Run the weekly steps, then:

7. **Research refresh.** The three prompts in [research-prompts.md](research-prompts.md) map to three file slugs under `research/`. For each slug find the newest `research/<slug>-YYYY-MM-DD.md`; its date is `<since>`. Launch the three prompts as background subagents at the same time (Claude Code: the Agent tool; Codex: spawn agents through the collaboration tools), each with `<since>`, `<today>` and `<previous file>` filled in, and continue with the weekly sections while they run. Each subagent writes `research/<slug>-<today>.md`. When one fails or has no web access, record that under `research` and continue with the previous file for the compliance pass.

8. **Compliance pass.** Read the newest research file per slug. Then check every own skill (each directory under `skills/` and `private/`) and `global/AGENTS.md` against the checklist below and write the `compliance` section: a table with one row per file and a verdict pill (`ok`, `proposal`, `finding`), then the proposals as diffs under `proposals`. Content changes are proposed, never applied.

Checklist for a skill:

- Description in third person, key use case first, what it does and when it applies, under 1,024 characters; the trigger cases match what usage shows the user reaching.
- `SKILL.md` body under 500 lines; references one level deep; substantial branch detail disclosed into `references/`.
- Register fit for Fable 5.1 and Astra: brief instructions rather than enumerated behaviours, no emphasis by capitals or "MUST", no anti-formatting language, lists only where items are parallel, user instructions stated as taking precedence where the skill could conflict with them.
- Categorical wording (`always`, `never`, `must`, `do not`) sits where a wrong guess costs something real: data, security, a destructive action, a decision the user has made. Each other instance is a finding; propose the intent or an overridable default in its place (the principle is in `writing-for-agents`, "Every document").
- Frontmatter uses only fields the harness reads (Agent Skills spec fields plus Claude Code's); a user-only skill has both `disable-model-invocation: true` and `policy.allow_implicit_invocation: false`; `agents/openai.yaml` `interface.short_description` is 25 to 64 characters and `default_prompt` names `$<name>`.
- Version references inside the skill (a CLI version the examples were checked against, a model id) match what is installed: `claude --version`, `codex --version`, `npm view skills version`.

Checklist for `global/AGENTS.md`:

- Under 200 lines; only what every session needs (commands the model cannot guess, conventions that differ from defaults, environment gotchas); multi-step procedures belong in a skill.
- Reads well for both harnesses (the same file is `~/.claude/CLAUDE.md` and `~/.codex/AGENTS.md`); emphasis on at most one line; categorical wording only where a wrong guess costs something real, as for skills above. Contradictions between rules are the consistency section's job.
- English, per the repo rule.

The `research` section holds the three new file paths, each with the headline changes since `<since>` in one or two sentences, or the failure. The monthly build uses `--eyebrow "manage-skills · monthly"` and `--chip "Kind=monthly"`.
