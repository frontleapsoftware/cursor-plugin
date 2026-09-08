# Frontleap Cursor Team Marketplace

This repository is a **Cursor Team Marketplace** only. It is not a single-root plugin repo.

Team marketplaces require this layout (see [fieldsphere/cursor-team-marketplace-template](https://github.com/fieldsphere/cursor-team-marketplace-template)):

```text
.cursor-plugin/marketplace.json
plugins/frontleap-admin/
  .cursor-plugin/plugin.json
  mcp.json
plugins/frontleap-client/
  .cursor-plugin/plugin.json
  mcp.json
LICENSE
README.md
```

Rules that matter for discovery:

- `metadata.pluginRoot` must be `"plugins"`
- each plugin `source` must be a bare directory name (`"frontleap-admin"`, `"frontleap-client"`), not `"."` or `"./frontleap-admin"`
- plugin files live under `plugins/<source>/`, not at the repo root

## Import in Cursor

1. **Dashboard → Plugins → Team Marketplaces → Add Marketplace → Import from Repo**
2. Use: `https://github.com/frontleapsoftware/cursor-plugin`
3. Cursor should detect **2 plugins: frontleap-admin and frontleap-client**
4. After merging marketplace changes, **Refresh** the marketplace (or re-import) so `marketplace.json` is rescanned
5. Install the plugin(s) you need, configure origins once per plugin, then connect MCP servers (Clerk OAuth)

If you previously installed the combined **frontleap** plugin, uninstall it and install **frontleap-admin** and/or **frontleap-client** instead.

## Which plugin to install

| Plugin | Install when you need… |
| --- | --- |
| **frontleap-admin** | Platform Admin MCP (admin tools across environments) |
| **frontleap-client** | Task Configuration / client MCP |

You can install one or both. Each plugin asks for the same three origin variables when you configure it.

## Plugin: frontleap-admin

Three HTTP MCP servers (admin × development / QA / production). Auth is Clerk OAuth at connect time — no tokens or secrets in this repo.

| Server | URL |
| --- | --- |
| `frontleap-dev-admin` | `${FRONTLEAP_DEV_URL}/mastra/api/mcp/admin/mcp` |
| `frontleap-qa-admin` | `${FRONTLEAP_QA_URL}/mastra/api/mcp/admin/mcp` |
| `frontleap-prod-admin` | `${FRONTLEAP_PROD_URL}/mastra/api/mcp/admin/mcp` |

## Plugin: frontleap-client

Three HTTP MCP servers (client × development / QA / production). Auth is Clerk OAuth at connect time — no tokens or secrets in this repo.

| Server | URL |
| --- | --- |
| `frontleap-dev-client` | `${FRONTLEAP_DEV_URL}/mastra/api/mcp/client/mcp` |
| `frontleap-qa-client` | `${FRONTLEAP_QA_URL}/mastra/api/mcp/client/mcp` |
| `frontleap-prod-client` | `${FRONTLEAP_PROD_URL}/mastra/api/mcp/client/mcp` |

### Configure

Configure origins once per installed plugin (scheme + host, no path, no trailing slash), for example:

- `FRONTLEAP_DEV_URL` → `https://dev.example.com`
- `FRONTLEAP_QA_URL` → `https://qa.example.com`
- `FRONTLEAP_PROD_URL` → `https://app.example.com`

Do not commit real customer hostnames or secrets.

## License

Apache-2.0 — see [LICENSE](./LICENSE).
