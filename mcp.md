# Remato MCP server

Remato exposes a [Model Context Protocol](https://modelcontextprotocol.io) server that lets AI assistants call the Remato external API as tools — list projects, manage tasks, query crew, pull time reports, and more.

## Endpoint

| | |
|---|---|
| URL | `https://api.ai-tools.remato.com/mcp` |
| Transport | Streamable HTTP |
| Auth | `x-api-key: <Remato admin API key>` |
| Health probe | `GET https://api.ai-tools.remato.com/mcp/health` (no auth) |
| Rate limit | 120 req/min per key |

The key must belong to an organization admin — non-admin keys are rejected at the MCP layer. Generate a key from **Settings → External API** in the Remato web app.

## Installation guides

Every config below points at the same URL with the same `x-api-key` header. Replace `<your-admin-api-key>` with your Remato admin API key.

### Claude Desktop

Edit `~/Library/Application Support/Claude/claude_desktop_config.json` (macOS) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "remato": {
      "type": "http",
      "url": "https://api.ai-tools.remato.com/mcp",
      "headers": {
        "x-api-key": "<your-admin-api-key>"
      }
    }
  }
}
```

Restart Claude Desktop. The Remato tools will appear in the tool picker.

### Claude Code

```sh
claude mcp add --transport http remato https://api.ai-tools.remato.com/mcp \
  --header "x-api-key: <your-admin-api-key>"
```

Verify with `claude mcp list`.

### Cursor

Create `~/.cursor/mcp.json` (global) or `.cursor/mcp.json` in the project root:

```json
{
  "mcpServers": {
    "remato": {
      "url": "https://api.ai-tools.remato.com/mcp",
      "headers": {
        "x-api-key": "<your-admin-api-key>"
      }
    }
  }
}
```

Cursor probes Streamable HTTP first and falls back to SSE automatically.

### VS Code (GitHub Copilot)

Edit `.vscode/mcp.json` (workspace) or your user-profile `mcp.json` (Command Palette → **MCP: Open User Configuration**):

```json
{
  "servers": {
    "remato": {
      "type": "http",
      "url": "https://api.ai-tools.remato.com/mcp",
      "headers": {
        "x-api-key": "${input:remato-api-key}"
      }
    }
  },
  "inputs": [
    {
      "type": "promptString",
      "id": "remato-api-key",
      "description": "Remato admin API key",
      "password": true
    }
  ]
}
```

Note: VS Code uses `servers` (not `mcpServers`). The first time the server starts, VS Code prompts for the key and stores it in the secret store.

### Windsurf

Edit `~/.codeium/windsurf/mcp_config.json`:

```json
{
  "mcpServers": {
    "remato": {
      "serverUrl": "https://api.ai-tools.remato.com/mcp",
      "headers": {
        "x-api-key": "<your-admin-api-key>"
      }
    }
  }
}
```

Note: Windsurf uses `serverUrl` (not `url`). After saving, hit the refresh button on the MCP toolbar in Cascade.

### Zed

Open the Zed settings file (`cmd+,`) and add a `context_servers` block:

```json
{
  "context_servers": {
    "remato": {
      "url": "https://api.ai-tools.remato.com/mcp",
      "headers": {
        "x-api-key": "<your-admin-api-key>"
      }
    }
  }
}
```

### Codex CLI

Edit `~/.codex/config.toml`:

```toml
[mcp_servers.remato]
url = "https://api.ai-tools.remato.com/mcp"
http_headers = { "x-api-key" = "<your-admin-api-key>" }
```

### Gemini CLI

Edit `~/.gemini/settings.json` (user) or `.gemini/settings.json` (project):

```json
{
  "mcpServers": {
    "remato": {
      "httpUrl": "https://api.ai-tools.remato.com/mcp",
      "headers": {
        "x-api-key": "<your-admin-api-key>"
      }
    }
  }
}
```

Note: Gemini CLI uses `httpUrl` (not `url`) for Streamable HTTP transport.

### ChatGPT

ChatGPT custom connectors require **OAuth 2.1 with Dynamic Client Registration** — static API-key auth is not supported. The Remato MCP server is not directly connectable from ChatGPT today.

### Other MCP clients

Anything that speaks Streamable HTTP MCP works — point it at `https://api.ai-tools.remato.com/mcp` and pass the `x-api-key` header.
