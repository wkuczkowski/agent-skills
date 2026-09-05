# MCP and structured output

Read the relevant section only when the task needs MCP or schema-validated output.

## MCP

Pass only the required configuration:

```bash
claude -p "<task using MCP>" \
  --model fable --effort medium \
  --permission-mode auto --permission-prompts none \
  --strict-mcp-config --mcp-config /absolute/path/mcp.json \
  --output-format json
```

- MCP tool names use `mcp__<server>__<tool>`.
- `mcp__<server>__*` matches tools on one server. Adding it to `--allowedTools` pre-approves those calls; do so only when that pre-approval is intended, not just to load the server.
- Prefer absolute command and argument paths in stdio server definitions.
- Without `--strict-mcp-config`, Claude loads account connectors and repository MCP configuration. A project `.mcp.json` can start a local command in print mode without an interactive trust dialog.

## Structured output

```bash
claude -p "Extract function names from auth.py" \
  --model fable --effort medium \
  --permission-mode auto --permission-prompts none \
  --strict-mcp-config --no-session-persistence --output-format json \
  --json-schema '{"type":"object","properties":{"functions":{"type":"array","items":{"type":"string"}}},"required":["functions"]}' \
  >structured.json 2>err.log
```

Check the actual exit and terminal fields using the run-status checks in SKILL.md before extracting `jq '.structured_output' structured.json. Read validated data from `.structured_output`. `.result` may contain a textual rendering of the same value, but it is not the structured-output field.
