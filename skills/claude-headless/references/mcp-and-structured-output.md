# MCP and structured output

Checked against Claude Code 2.1.316 and https://code.claude.com/docs/en/headless on 2026-11-07.

## MCP

Load only the servers the task needs: `--strict-mcp-config --mcp-config /absolute/path/mcp.json`, with absolute command and argument paths in stdio definitions. Without `--strict-mcp-config`, a `-p` run loads account connectors and the repository's `.mcp.json`, and starts a project server's local command without any trust dialog or per-server approval, even in a folder never trusted.

- Tool names are `mcp__<server>__<tool>`; `mcp__<server>__*` matches one server. Putting that in `--allowedTools` pre-approves the calls, so add it only when pre-approval is intended, not to load the server.
- Entries that fail validation are skipped and the run still exits cleanly. With stderr redirected there is no warning; the skipped entries are only in `system/init.mcp_server_errors` (stream-json), and loaded servers with their status in `mcp_servers`.
- A `-p` run does not wait for MCP work in the background: a call that was moved to the background loses its result.

## Structured output

`--output-format json --json-schema '<schema>'` puts the validated value in `.structured_output`. When the run ends without valid output, for example because Claude declined, the field is absent, so check for it after the run-status checks in SKILL.md. `.result` may carry a textual rendering of the same value; it is not the validated field. An invalid schema stops the CLI before the run with `Error: --json-schema is not a valid JSON Schema`. `format` keywords are accepted but not enforced.
