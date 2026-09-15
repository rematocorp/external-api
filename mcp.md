# Remato MCP server

Remato exposes a [Model Context Protocol](https://modelcontextprotocol.io) server that lets AI assistants call the Remato external API as tools — list projects, manage tasks, query crew, pull time reports, and more.

Instead of copy-pasting data into a chat, the AI can securely query systems directly and act on live information.

## Endpoint

| | |
|---|---|
| URL | `https://api.ai-tools.remato.com/mcp` |
| Transport | Streamable HTTP |
| Auth | `x-api-key: <Remato admin API key>` or `OAuth` |
| Health probe | `GET https://api.ai-tools.remato.com/mcp/health` (no auth) |
| Rate limit | 100 req/min per key |

The key must belong to an organization admin — non-admin keys are rejected at the MCP layer. Contact Remato Support for API access.

## Installation guides

Claude, ChatGPT and other tool installation guides are avaliable here: https://remato.notion.site/mcp
