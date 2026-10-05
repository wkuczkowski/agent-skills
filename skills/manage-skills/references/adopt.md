# Adopting a vendor skill

Adoption turns `.agents/skills/<name>` (vendor, verbatim, tracked by `npx skills`) into `skills/<name>` (own, rewritten for this user, pinned to the upstream commit it started from). The name stays; the text is the agent's own, keeping what the vendor skill got right.

## The rewrite

The agent drafts the rewrite itself from the vendor skill, its usage (`usage.yaml`, `bin/usage`), what upstream changed (`bin/upstream --diff <name>`), and what the user's sessions show about how he uses it, following `writing-for-agents`. It puts to him only the decisions that are his, such as what the skill should be for and anything his sessions do not settle, and writes after his go-ahead. On 2026-10-05 `domain-modeling` was adopted this way with one clarification from him (the glossary records what he means by his terms, not what the agent decides they mean). Until then adoption ran a three-round interview, and most answers accepted the recommendation.

## Mechanics

The order matters here, since `bin/upstream` parses the manifest form exactly and `npx skills remove` would delete the new directory.

1. The commit to pin: `gh api "repos/<owner/repo>/commits?path=<skill dir>&per_page=1" --jq '.[0].sha'`, with owner/repo and the path from the skill's `skills-lock.json` entry. It goes in the frontmatter as `metadata.upstream`, `metadata.upstream-commit` and `metadata.adopted`.
2. The manifest entry replaces the vendor one:

   ```yaml
   <name>:
     kind: own
     adopted: true
     upstream: <owner/repo> <skillPath>@<sha>
   ```

3. `bin/unvendor <name>`, then `bin/link` and `bin/link check` (0 findings), and `bin/upstream` lists the skill as `own`, `unchanged`.
4. The rest of "A skill here is done when" in `SKILL.md`, including the test in both harnesses.
