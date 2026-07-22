# agent-skills

Personal agent skills, installable with [`npx skills`](https://github.com/vercel-labs/skills):

```bash
npx skills add wkuczkowski/agent-skills -g -a '*'
```

## Skills

- **claude-headless** — run Claude Code programmatically via the headless CLI (`claude -p`): output parsing, session resume, permissions, structured output, verified gotchas.
- **codex-headless** — run OpenAI Codex CLI programmatically via `codex exec`: gpt-\*-sol model + reasoning-effort selection, workspace-write sandbox, "Approve for me" auto-review, JSONL/schema output, session resume.
