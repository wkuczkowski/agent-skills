# MCP servers

Read this file only when the Cursor task requires an MCP server.

## Configure

There is no `--mcp-config` flag. Configuration comes only from `.cursor/mcp.json` in the workspace or from `~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "my-server": {
      "type": "stdio",
      "command": "/abs/path/to/cmd",
      "args": ["--directory", "/abs/path/to/project", "..."]
    }
  }
}
```

Use absolute paths. Cursor CLI does not interpolate `${userHome}` or `${workspaceFolder}`.

## Approve and verify

1. From the repository root, run `cursor-agent mcp enable my-server`. Until then `cursor-agent mcp list` prints `my-server: not loaded (needs approval)`. A scripted run with `--approve-mcps` reached the server without `mcp enable`; that flag approves every configured server, so pass it only when the task requires them.
2. Add narrow per-tool allow rules to `.cursor/cli.json` when those calls should bypass individual classifier review, for example `"allow": ["Mcp(my-server:tool_a)", "Mcp(my-server:tool_b)"], "deny": []`. Both arrays are required. Whether Auto Review blocks MCP calls without such a rule is unverified.
3. Run `cursor-agent mcp list` and expect `my-server: ready`.
4. Run `cursor-agent mcp list-tools my-server` and confirm the required tools and arguments exist.

Without server approval, the headless agent sees no tools from that server.

## In the stream

- Cursor loads MCP schemas lazily: each MCP call is preceded by a `getMcpToolsToolCall` with `args.server` and `args.toolName`.
- The call itself is a `tool_call` holding `mcpToolCall`; the server is `args.providerIdentifier` and the tool `args.toolName`.
- The server's own response envelope is a JSON string inside `.result.success`; parse it before reading its fields.
