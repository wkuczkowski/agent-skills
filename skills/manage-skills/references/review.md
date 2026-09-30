# Review

Weekly: usage, unused candidates, candidates from conversations, upstream changes, drift, consistency, proposals. Monthly: the same plus a research refresh, a compliance pass, and consistency widened to vendor skills.

The user reads a short message and practically never a long report. The deliverable is `reports/<date>/review.md` (`<date>` from `date +%F`), written as that message: in an interactive session it is also the chat reply; in a headless run the notification points to it. Beside it stay the raw command outputs, so every number can be traced, and one `.diff` file per proposal, so the agent that applies an accepted proposal has it ready. The review proposes; the only files written are `usage.yaml` (by `bin/usage --write`), the report directory, `reports/.workflow-from-chats-last-run` and, monthly, the new research files.

A finding is written from this run's command output; one written from memory or from a previous review does not count. When the user says the data files for today already exist, steps 1 to 3 are read from those files instead of re-run.

## Weekly steps

Run from the repo. `D=reports/$(date +%F)`; `mkdir -p "$D"`.

1. `bin/usage --write > "$D/usage.txt"`, then `bin/usage --unused > "$D/unused.txt"`.
2. `bin/upstream --json > "$D/upstream.json"` (exit 1 means at least one skill changed; that is a result, not an error). For each record with `status: changed`: `bin/upstream --diff <name> > "$D/upstream-<name>.diff"` and read the diff.
3. `bin/link check > "$D/check.txt"` (exit 1 whenever there are findings; a clean check reports 0).
4. Candidates from conversations: when `"$D/conversations.md"` does not exist yet, read `skills/workflow-from-chats/SKILL.md` and carry out that skill with its default window (since its last run). It is user-only, so it is followed by reading the file, not through the Skill tool. Its working file lands at `"$D/conversations.md"`; when the file already exists from today, use it as it is.
5. Consistency: read together everything an agent loads together and note each contradiction in `"$D/consistency.txt"` (file and line for both sides, the kind, the proposed resolution). Weekly scope: every own skill under `skills/` and `private/`, `global/AGENTS.md` and the repo's `AGENTS.md`, since these change often. Monthly scope: also every vendor skill a harness sees (`bin/link list claude-code`, `bin/link list codex`). A contradiction is one of four kinds: two skills claiming the same trigger (compare descriptions first, then bodies); a skill contradicting a rule in the global instructions; one rule stated twice with different wording; a reference to a path, flag or command that no longer exists (confirm with `ls`, `--help` or the harness). Done when every file in scope has been read in this run and each finding carries both quotes.
6. Write `"$D/review.md"` in the form below and one `"$D/P-<nn>.diff"` per proposal that changes a file. Done when every proposal in the message has its diff or command.

## The message

In the user's language (usually Polish), short enough to read at once, numbered so he can answer "P-02 tak, P-04 nie":

- One line: weekly or monthly, the usage window, and the counts: unused candidates, conversation proposals, upstream changes, drift findings, consistency findings.
- Proposals, most useful first, each `P-<nn>` with the target and one or two plain sentences on what would change and why: retirement candidates (the rule: six weeks without invocation for a model-invocable skill, twelve for a user-only one, `keep: true` exempt; name skills whose only Codex use falls on days of work on this repo as possibly unused, since `bin/usage` counts reading a `SKILL.md` as use), conversation proposals (carried over from `"$D/conversations.md"` with their grade), upstream changes worth taking (what changed in substance, and for a vendor skill with `local_drift` that updating loses the edits), drift fixes, consistency fixes (both quotes in short, which wins), monthly compliance findings.
- Open decisions for him, one sentence each.
- One line on what failed or was not checked, and monthly the three research files.
- Last line: the directory with the data files and diffs.

No tables of all skills and no full diffs in the message; those stay in the files.

The commands behind the proposals: retirement `bin/unvendor <name>` for vendor, `git rm -r skills/<name>` for own, plus the manifest entry; vendor update `npx skills update <name>`; adopted skills whose upstream changed: the edits worth carrying over, as a diff against `skills/<name>`. A vendor skill is not edited; a consistency fix there is a manifest narrowing, an adoption or a line in an own file.

## Monthly additions

Run the weekly steps, then:

7. **Research refresh.** The three prompts in [research-prompts.md](research-prompts.md) map to three file slugs under `research/`. For each slug find the newest `research/<slug>-YYYY-MM-DD.md`; its date is `<since>`. Launch the three prompts as background subagents at the same time (Claude Code: the Agent tool; Codex: spawn agents through the collaboration tools), each with `<since>`, `<today>` and `<previous file>` filled in, and continue with the weekly sections while they run. Each subagent writes `research/<slug>-<today>.md`. When one fails or has no web access, note it in the message and continue with the previous file for the compliance pass.

8. **Compliance pass.** Read the newest research file per slug. Then check every own skill (each directory under `skills/` and `private/`) and `global/AGENTS.md` against the checklist below; write the verdict per file (`ok`, `proposal`, `finding`) to `"$D/compliance.txt"` and carry the proposals into the message with their diffs. Content changes are proposed, never applied.

Checklist for a skill:

- Description in third person, key use case first, what it does and when it applies, under 1,024 characters; the trigger cases match what usage shows the user reaching.
- `SKILL.md` body under 500 lines; references one level deep; substantial branch detail disclosed into `references/`.
- Register fit for Fable 5.1 and Astra: brief instructions rather than enumerated behaviours, no emphasis by capitals or "MUST", no anti-formatting language, lists only where items are parallel, user instructions stated as taking precedence where the skill could conflict with them.
- The line describes (a fact about the machine, the user, the repo, or what has worked) rather than commands, except where a wrong guess costs something real; a line that tells a Fable 5.1 or Astra class model how to do ordinary work is a finding.
- Categorical wording (`always`, `never`, `must`, `do not`) sits where a wrong guess costs something real: data, security, a destructive action, a decision the user has made. Each other instance is a finding; propose the intent or an overridable default in its place (the principle is in `global/AGENTS.md`, "The user" and "How the work tends to go").
- Frontmatter uses only fields the harness reads (Agent Skills spec fields plus Claude Code's); a user-only skill has both `disable-model-invocation: true` and `policy.allow_implicit_invocation: false`; `agents/openai.yaml` `interface.short_description` is 25 to 64 characters and `default_prompt` names `$<name>`.
- Version references inside the skill (a CLI version the examples were checked against, a model id) match what is installed: `claude --version`, `codex --version`, `npm view skills version`.

Checklist for `global/AGENTS.md`:

- Under 200 lines; only what every session needs (commands the model cannot guess, conventions that differ from defaults, environment gotchas); multi-step procedures belong in a skill.
- Reads well for both harnesses (the same file is `~/.claude/CLAUDE.md` and `~/.codex/AGENTS.md`); emphasis on at most one line; categorical wording only where a wrong guess costs something real, as for skills above. Contradictions between rules belong to the consistency step.
- English, per the repo rule.

The message names the three new research files, each with its headline change since `<since>` in one sentence, or the failure.
