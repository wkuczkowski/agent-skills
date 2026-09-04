# MCP servers

Read this file only when the Codex task requires an MCP server.

```toml
[mcp_servers.my-server]
command = "uv"
required = true
default_tools_approval_mode = "approve"
args = ["--directory", "/abs/path/to/project", "run", "python", "-m", "my_server.main"]
```

- Put global configuration in `~/.codex/config.toml` or project configuration in `.codex/config.toml` within a trusted project.
- Set `required = true` when the task cannot succeed without the server. Codex then fails at startup instead of silently continuing.
- Set `default_tools_approval_mode = "approve"` only for a server whose tools are trusted for this task.
- Prefer absolute paths in `args`. Relative paths resolve against the launch directory.
- Keep the normal Auto-review flags on the `codex exec` command.
