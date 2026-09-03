<p align="center">
  <a href="https://www.mural.co">
    <img src="assets/logo.svg" width="96" alt="Mural" />
  </a>
  <h1 align="center">Agent Toolkit</h1>
</p>

Connect AI agents to [Mural](https://www.mural.co).

---

## Features

- **Mural MCP server** — Edit your murals and create new content directly from your agent.
- **Cursor plugin** — Available on the Cursor Marketplace.
- **Agent Plugin** — the root `plugin.json` follows the vendor-neutral [Agent Plugins](https://agent-plugins.org) standard.

---

## Install

### Cursor plugin

1. Open **Cursor Settings → Plugins**.
2. Search for **Mural**.
3. Click **Install**, then complete the Mural sign-in prompt in your browser.

Or run `/add-plugin mural` in chat.

---

## Any MCP client

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

Mural MCP uses OAuth. There are no tokens or API keys to configure — on first connect your client opens a browser, you sign in with your Mural account, and the agent then acts on your behalf. Access is scoped to the murals your Mural user can already reach.

---

## Local development

To test the plugin before submitting:

#### Cursor

```bash
cp -R . ~/.cursor/plugins/local/mural
```

Restart Cursor, then confirm the plugin appears under **Customize** and its tools resolve against the live server.


