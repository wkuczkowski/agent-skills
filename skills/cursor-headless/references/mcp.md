# MCP servers

Read this file only when the Cursor task requires an MCP server.

## Configure

Put project configuration in `.cursor/mcp.json` or global configuration in `~/.cursor/mcp.json`:

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

1. From the repository root, run `cursor-agent mcp enable my-server`. For a one-off scripted run, use `--approve-mcps` only when the task requires that server.
2. Add a narrow `Mcp(my-server:*)` allow rule to `.cursor/cli.json` when its tool calls should bypass individual classifier review. Keep both `allow` and `deny` arrays in that file.
3. Run `cursor-agent mcp list` and expect `my-server: ready`.
4. Run `cursor-agent mcp list-tools my-server` and confirm the required tools and arguments exist.

Without server approval, the headless agent sees no tools from that server.
