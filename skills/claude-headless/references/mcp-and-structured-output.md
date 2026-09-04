# MCP and structured output

Read the relevant section only when the task needs MCP or schema-validated output.

## MCP

Pass only the required configuration:

```bash
claude -p "<task using MCP>" \
  --model fable --effort high \
  --permission-mode auto --permission-prompts none \
  --strict-mcp-config --mcp-config /absolute/path/mcp.json \
  --allowedTools "mcp__my-server__*" \
  --output-format json
```

- MCP tool names use `mcp__<server>__<tool>`.
- Use `mcp__<server>__*` for every tool on one explicitly selected server.
- Prefer absolute command and argument paths in stdio server definitions.
- Without `--strict-mcp-config`, Claude loads account connectors and repository MCP configuration. A project `.mcp.json` can start a local command in print mode without an interactive trust dialog.

## Structured output

```bash
claude -p "Extract function names from auth.py" \
  --model fable --effort high \
  --permission-mode auto --permission-prompts none \
  --strict-mcp-config --no-session-persistence --output-format json \
  --json-schema '{"type":"object","properties":{"functions":{"type":"array","items":{"type":"string"}}},"required":["functions"]}' \
  | jq '.structured_output'
```

Read validated data from `.structured_output`. `.result` may contain a textual rendering of the same value, but it is not the structured-output field.
