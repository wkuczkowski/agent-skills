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
- `required = true` makes Codex fail at startup when the server does not come up, instead of continuing without it. Since `0.172.0` (`#62366`), optional (`required = false`/default) `stdio` servers with cached tool lists lazy-load on their first tool call instead of at startup, so set `required = true` when a task must fail fast if the server cannot start.
- `default_tools_approval_mode = "approve"` only for a server whose tools are trusted for this task.
- Relative paths in `args` resolve against the launch directory.
