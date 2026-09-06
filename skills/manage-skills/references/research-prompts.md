# Research prompts

Three prompts for the monthly research refresh. Each is derived from the questions the first research file of its topic answered (`research/<slug>-2026-09-06.md`), narrowed to changes since the previous file. Fill in `<since>` (the date in the newest existing file name for the slug), `<today>` (`date +%F`) and `<previous file>` (that file's path), then run each prompt as a background subagent with web access. Every prompt writes one file, `research/<slug>-<today>.md`, and changes nothing else.

Common preamble, prepended to each prompt:

```
You are refreshing a research note for the repo at /home/wkuczkowski/projects/skills.
Read <previous file> first; it answers the questions below as of <since>. Your job is to
report what changed since <since>: new facts, corrected facts, removed features, new
versions. Confirm every claim against a primary source (official docs, source code,
release notes, the installed CLI) and cite it with a URL or file path and, where the
page shows one, its date or version. Record the installed versions you checked against.
Where something could not be confirmed from a primary source, say so in a closing
section "Not confirmed". Keep the previous file's section structure so the two can be
compared. Write the result to research/<slug>-<today>.md and nothing else; do not edit
any other file. Write in English.
```

## Slug `claude-code-skills-and-fable`

Anthropic, Claude Code and Claude Fable. Questions:

```
1. Claude Code skills. Since <since>: changes to the SKILL.md format and the supported
   frontmatter fields; string substitutions and dynamic context; discovery locations
   (including whether Claude Code now reads .agents/skills or ~/.agents/skills); what is
   loaded at session start versus on invocation, and the description length cap; how
   disable-model-invocation and user-invocable change loading; /skill-doctor,
   `claude plugin validate` and `claude plugin eval`. Check against the installed
   `claude --version`.
2. Official guidance for writing skills and CLAUDE.md. Since <since>: the Agent Skills
   best-practices page, the Claude Code skills and memory pages, the Agent Skills
   specification (agentskills.io) as linked from Anthropic's docs; description writing,
   length limits, progressive disclosure, what belongs in CLAUDE.md, size guidance.
3. Prompting guidance for the current Claude models. Since <since>: new model releases
   and their ids, the shared prompting best-practices page, the per-model pages for the
   newest Fable model, migration notes, and any statement about skills written for older
   models being too prescriptive; the recommended register (brief instructions, no
   aggressive emphasis), formatting and verbosity guidance, effort levels.
4. Hooks. Since <since>: PreToolUse hooks on Bash, their input and output fields,
   additionalContext or equivalent, and whether a skill's frontmatter can register hooks.
```

## Slug `codex-skills-and-astra`

OpenAI, Codex CLI and GPT Astra. Questions:

```
1. Codex skills. Since <since>: SKILL.md fields Codex parses and their limits; the
   agents/openai.yaml schema (interface, policy.allow_implicit_invocation, dependencies);
   discovery roots and their order, including .agents/skills, ~/.agents/skills,
   ~/.codex/skills and plugin roots; symlink handling for skill directories and for a
   symlinked SKILL.md; the session-start skills catalog and its size cap; explicit
   $skill invocation; where the skills docs live now. Check against the installed
   `codex --version` and the bundled skills under ~/.codex/skills/.system.
2. Official guidance for writing skills and AGENTS.md. Since <since>: the build-skills
   page, the bundled skill-creator's principles and validator, the AGENTS.md page
   (discovery, merge order, size cap, trust), and the skills versus AGENTS.md versus
   plugins versus hooks guidance.
3. The current default Codex model. Since <since>: its official name and API id, release
   notes, supported reasoning efforts in the API and in Codex, and the official prompting
   guide: instruction following and sensitivity to skill files, instruction priority
   between user and skill, formatting and verbosity, delegation, testing, recommended
   prompt snippets. Note what changed in the Codex release notes for this model.
4. Hooks. Since <since>: whether a hook fires before a shell command, its input, and
   whether it can deny, rewrite or add model-visible guidance; configuration surfaces.
```

## Slug `skills-cli-and-cursor`

The `skills` CLI (vercel-labs) and Cursor, comparison only. Questions:

```
1. The npm package `skills`. Since <since> (previous version in <previous file>): the
   current version and its date; project versus global scope, canonical directories and
   lock file paths; which agents are "universal" and which get symlinks, in particular
   claude-code, codex and cursor; whether a local filesystem path can be a source and
   whether files are copied or symlinked; `--copy`.
2. `skills update` in both scopes: what is compared, whether local edits or extra files
   inside an installed skill survive, and what `sourceType: local` entries do.
3. `remove`, `experimental_install` and `list`: what each touches, including the lock
   entry and the canonical directory.
4. Lock file formats for skills-lock.json and the global lock, field by field, and how
   computedHash is calculated.
5. Behaviour when hand-written skills sit next to CLI-managed ones in .agents/skills.
6. Cursor: skill and rule locations, frontmatter fields, AGENTS.md support, and anything
   that changed in how Cursor reads the Claude and Codex directories.
```
