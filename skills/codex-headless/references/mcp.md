# MCP servers

Read this when the Codex task needs an MCP server.

```toml
[mcp_servers.my-server]
command = "uv"
required = true
default_tools_approval_mode = "approve"
args = ["--directory", "/abs/path/to/project", "run", "python", "-m", "my_server.main"]
```

- Global configuration goes in `~/.codex/config.toml`, project configuration in `.codex/config.toml` of a trusted project.
- `required = true` makes Codex fail at startup when the server does not come up, instead of continuing without it.
- `default_tools_approval_mode = "approve"` only for a server whose tools are trusted for this task.
- Relative paths in `args` resolve against the launch directory.
