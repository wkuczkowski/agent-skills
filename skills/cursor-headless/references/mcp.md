# MCP servers

Read only when the Cursor task needs an MCP server. Measured on 2026.09.18; flags and `mcp` subcommands re-checked on 2026.10.22.

## Configure and approve

- No `--mcp-config` flag: servers come only from `.cursor/mcp.json` in the workspace or `~/.cursor/mcp.json` (`{"mcpServers": {"name": {"type": "stdio", "command": ..., "args": [...]}}}`).
- Use absolute paths. `${userHome}` and `${workspaceFolder}` were not interpolated on 2026.09.18. (The 2026.10.08 changelog mentions `${VAR}` in OAuth client ids, so some interpolation may exist now; not tested.)
- A configured server is `not loaded (needs approval)` in `cursor-agent mcp list` until `cursor-agent mcp enable <name>` from the repository root, or until the run passes `--approve-mcps`, which approves every configured server; pass it only when the task needs them. Without approval the headless agent sees no tools from that server.
- Narrow per-tool allow rules (`Mcp(server:tool)` in `.cursor/cli.json`, both `allow` and `deny` arrays present) bypass classifier review; changing them is the user's call. Auto Review leans toward allowing MCP calls that only read or draft and blocking ones that send, post or delete (2026.09.22 changelog).
- Check with `cursor-agent mcp list` (expect `ready`) and `cursor-agent mcp list-tools <name>`. Since 2026.10.22 one invalid tool no longer hides a server's other tools, and a server that fails to list tools reports an error to the agent.

## In the stream

- Schemas load lazily: each MCP call is preceded by a `getMcpToolsToolCall` with `args.server` and `args.toolName`.
- The call is a `tool_call` holding `mcpToolCall`, server in `args.providerIdentifier`, tool in `args.toolName`.
- The server's response envelope is a JSON string inside `.result.success`; parse it before reading fields.
