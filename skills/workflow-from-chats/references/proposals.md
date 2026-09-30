# Proposals: the reply and the working file

## The chat reply

What the user reads and answers. Short enough to take in at once, numbered so he can answer "P-02 yes, P-04 no".

- One opening line: the window, conversations and human turns examined per harness.
- One entry per strong or medium proposal, most useful first: id (`P-01`, ...), kind (new skill, skill edit, instruction change) and target, then one or two plain sentences on what would change and why, with the evidence as a count and one short quote when a quote makes the point.
- Contradicted clusters as questions for him, both sides in one sentence each.
- Dismissed clusters in one line, only the ones he might expect to see.
- One coverage line: what was excluded or unavailable, and whether client data was set aside.
- Last line: the path of the working file.

No diffs, no tables of evidence, no transcript paths in the reply.

## The working file

`reports/<date>/conversations.md`, for agents. Sections: window and coverage (per harness: roots, conversations, turns kept; exclusions with counts; unavailable or malformed sources), proposals, decisions, dismissed, evidence.

Per proposal: id, title, grade, kind, the evidence ids for and against, and the change itself:

- **New skill:** everything `manage-skills new` needs: `name` (lowercase letters, digits, hyphens, equal to the directory); `description` (third person, what it does then when it applies, under 400 characters, no angle brackets); invocation (`model-invocable` or `user-only`, with the reason); harnesses (`both`, `claude-code` or `codex`, with the reason when narrowed); `short_description` for `agents/openai.yaml` (25 to 64 characters); a draft `SKILL.md`.
- **Skill edit** and **instruction change:** a unified diff made with `diff -u` from the current file, so it applies.

Evidence entries: id, harness, absolute transcript path, locator (line, plus message uuid for Claude Code or message id for Codex), local timestamp, a short excerpt.
