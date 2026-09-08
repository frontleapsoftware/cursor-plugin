# Frontleap Cursor plugin

Cursor **team marketplace** that ships the Frontleap plugin: MCP servers for **development**, **QA**, and **production**. One install gives you six HTTP MCP servers (admin + client per environment). Auth is Clerk OAuth at connect time — no tokens or secrets belong in this repo or in `mcp.json`.

## Team marketplace layout

Cursor team marketplaces require a multi-plugin layout. This repo uses:

```text
.cursor-plugin/marketplace.json          # marketplace manifest (lists plugins)
plugins/frontleap/
  .cursor-plugin/plugin.json             # plugin manifest, author, variables
  mcp.json                               # six MCP server URL definitions
README.md
LICENSE
```

- `metadata.pluginRoot` is `plugins`.
- Each plugin `source` is a bare directory name under that root (e.g. `frontleap`), not `.` or `./frontleap`.

## Import / refresh in Cursor

1. In Cursor **Dashboard → Plugins**, add this repository as a **Team Marketplace** (or re-import if it was already added).
2. After merging layout changes, use **Refresh** on the team marketplace so Cursor rescans `marketplace.json`.
3. Install the **frontleap** plugin from the marketplace.
4. Open **Plugins → Configure** for **frontleap**.
5. Set the three public origins (scheme + host only, no path, no trailing slash), for example:
   - `FRONTLEAP_DEV_URL` → `https://dev.example.com`
   - `FRONTLEAP_QA_URL` → `https://qa.example.com`
   - `FRONTLEAP_PROD_URL` → `https://app.example.com`
6. Connect each MCP server you need; Cursor prompts for Clerk OAuth. Do not paste Authorization headers or API keys into plugin config.

If the marketplace shows **No plugins found**, confirm the repo has `.cursor-plugin/marketplace.json` with `pluginRoot: "plugins"` and that `plugins/<name>/.cursor-plugin/plugin.json` exists, then refresh or re-import.

## MCP servers

| Server | URL |
| --- | --- |
| `frontleap-dev-admin` | `${FRONTLEAP_DEV_URL}/mastra/api/mcp/admin/mcp` |
| `frontleap-dev-client` | `${FRONTLEAP_DEV_URL}/mastra/api/mcp/client/mcp` |
| `frontleap-qa-admin` | `${FRONTLEAP_QA_URL}/mastra/api/mcp/admin/mcp` |
| `frontleap-qa-client` | `${FRONTLEAP_QA_URL}/mastra/api/mcp/client/mcp` |
| `frontleap-prod-admin` | `${FRONTLEAP_PROD_URL}/mastra/api/mcp/admin/mcp` |
| `frontleap-prod-client` | `${FRONTLEAP_PROD_URL}/mastra/api/mcp/client/mcp` |

## URL contract

Frontleap MCP endpoints use a fixed path under each app origin:

```text
{APP_URL}/mastra/api/mcp/{admin|client}/mcp
```

- **admin:** `/mastra/api/mcp/admin/mcp`
- **client:** `/mastra/api/mcp/client/mcp`

Dashboard variables supply only the public origin. The plugin appends the fixed path. Transport is HTTP / streamable HTTP via the Cursor `url` field only.

## Configuration notes

- Variables are **required** strings: `FRONTLEAP_DEV_URL`, `FRONTLEAP_QA_URL`, `FRONTLEAP_PROD_URL`.
- Values must be origin only (`https://host`) — no path segment and no trailing slash.
- Do not commit real customer hostnames or secrets to this repository.
- Authentication is handled by Clerk OAuth when you connect a server in Cursor.

## License

Apache-2.0 — see [LICENSE](./LICENSE).
