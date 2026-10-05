# Research refresh

Three notes under `research/`, one per slug, each refreshed by a background subagent with web access. The newest `research/<slug>-YYYY-MM-DD.md` is the baseline; the refresh reports what changed since its date (new, corrected or removed facts, new versions), confirmed against primary sources (official docs, source code, release notes, the installed CLI) and cited, with the installed versions checked against and a closing "Not confirmed" section. It keeps the baseline's section structure so the two compare, writes only `research/<slug>-<today>.md`, in English.

- `claude-code-skills-and-fable`: Claude Code skills (format, frontmatter, discovery, loading, invocation flags, validation and eval tools), Anthropic's guidance for skills and instruction files (AGENTS.md support), prompting guidance for the current Claude models, and hooks.
- `codex-skills-and-astra`: Codex skills (fields, `agents/openai.yaml`, discovery roots, symlinks, the catalog and its cap, `$skill`), OpenAI's guidance for skills and AGENTS.md, the current default Codex model and its prompting guide, and hooks.
- `skills-cli-and-cursor`: the `skills` npm CLI (scopes, directories, lock files and `computedHash`, `update`, `remove`, `check`, hand-written skills next to managed ones) and how Cursor reads skills, rules and AGENTS.md.
