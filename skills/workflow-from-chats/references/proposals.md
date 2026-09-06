# Proposals and the report

`reports/<date>/conversations.html`, built with `assets/report/build` from a sections fragment (components and build options: `assets/report/README.md`). Header: title `Conversations <date>`, eyebrow `workflow-from-chats`, chips for the window start and end, the sources examined, and the proposal count. Sections in this order; a section with nothing in it says `none` and why, and stays.

## Sections

1. `summary`: a `brief` ledger with the window, the number of conversations examined per harness, the number of human turns kept, and the proposals per grade; then the proposals as a list of links.
2. `coverage`: what was examined (per harness: transcript roots, conversation count, turn count), what was excluded (automated runs, subagent threads, uncertain authorship, set-aside material) with counts, and which sources were unavailable or malformed.
3. `proposals`: one card per strong or medium proposal, in the forms below.
4. `decisions`: contradicted clusters, both sides quoted, the decision the user has to make.
5. `dismissed`: weak clusters and clusters covered by an existing skill: one line each with the reason or the covering skill.
6. `evidence`: the index of cited turns, one entry per turn, in the evidence-link form from `evidence.md`, with an `id` each proposal refers to.
7. `method`: window, last-run file value read, commands used, checks performed and checks not performed.

## Proposal forms

Every card carries a stable id (`P-01`, ...), a title, a grade pill, one sentence on why it is worth adopting, the evidence ids for and against, a realistic future request that would exercise it, and the observable result that shows it works. The remaining content depends on the kind.

**New skill.** Everything `manage-skills new` needs, so the user can hand the card over unchanged:

- `name`: lowercase letters, digits and hyphens, equal to the directory.
- `description`: third person, what it does then when it applies, key use case first, one trigger per branch, under 400 characters, no angle brackets.
- invocation: `model-invocable` or `user-only`, with the reason (user-only when only the user should start it, typically a workflow with side effects).
- harnesses: `both`, `claude-code` or `codex`, with the reason when narrowed.
- `short_description` for `agents/openai.yaml`: 25 to 64 characters.
- draft `SKILL.md` in a `details` block: frontmatter plus a body that states the steps, each ending on a checkable criterion, with branch material named for `references/`.

**Skill edit.** The skill's name and path, current behaviour versus proposed behaviour in one sentence each, and a unified diff against the current file in `<pre class="diff">`, produced from the file as it is now so it applies.

**Instruction change.** A unified diff against `global/AGENTS.md` (or the repo `AGENTS.md` when the rule is repo-specific) in `<pre class="diff">`, produced with `diff -u` from the current file.

A proposal is styled `proposed`; nothing in the report reads as applied.
