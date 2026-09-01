<p align="center">
  <a href="https://www.mural.co">
    <img src="assets/logo.svg" width="96" alt="Mural" />
  </a>
  <h1 align="center">Mural AI Manifests</h1>
</p>

Plugin manifests that connect AI agents to [Mural](https://www.mural.co) through Mural's
remote [Model Context Protocol](https://modelcontextprotocol.io) server.

This repository contains configuration only — the MCP server itself is hosted by Mural at
`https://mcp-canvas.mural.co/mcp`.

---

## Features

- **MCP server** — create and edit murals, place and update widgets, apply templates, and
  search workspaces directly from your agent.
- **Cursor plugin** — available on the Cursor Marketplace.
- **Agent Plugin** — the root `plugin.json` follows the vendor-neutral
  [Agent Plugins](https://agent-plugins.org) standard, so the same repo can serve other
  compatible clients as we add them.

---

## Install

### Cursor

1. Open **Cursor Settings → Plugins**.
2. Search for **Mural**.
3. Click **Install**, then complete the Mural sign-in prompt in your browser.

Or run `/add-plugin mural` in chat.

### Any MCP client

Point your client at the remote server:

```json
{
  "mcpServers": {
    "Mural": {
      "type": "http",
      "url": "https://mcp-canvas.mural.co/mcp"
    }
  }
}
```

---

## Authentication

The server uses OAuth. There are no tokens or API keys to configure — on first connect your
client opens a browser, you sign in with your Mural account, and the agent then acts as you.
Access is scoped to the workspaces and murals your Mural user can already reach.

---

## Repository layout

```
.
├── plugin.json            # Agent Plugin manifest (portable standard)
├── mcp.json               # Agent Plugin MCP config (streamable-http)
├── .mcp.json              # MCP config for Cursor (http)
├── .cursor-plugin/        # Cursor manifest
└── assets/                # Logo
```

---

## Local development

To test the plugin before submitting:

```bash
cp -R . ~/.cursor/plugins/local/mural
```

Restart Cursor, then confirm the plugin appears under **Customize** and its tools resolve
against the live server.

---

## Support

- Docs: https://developers.mural.co
- Issues: https://github.com/tactivos/agent-toolkit/issues

## License

MIT
