# Frontleap Cursor Team Marketplace

This repository is a **Cursor Team Marketplace** only. It is not a single-root plugin repo.

Team marketplaces require this layout (see [fieldsphere/cursor-team-marketplace-template](https://github.com/fieldsphere/cursor-team-marketplace-template)):

```text
.cursor-plugin/marketplace.json          # marketplace registry
plugins/frontleap/
  .cursor-plugin/plugin.json             # plugin manifest + variables
  mcp.json                               # MCP server URLs (OAuth only)
LICENSE
README.md
```

Rules that matter for discovery:

- `metadata.pluginRoot` must be `"plugins"`
- each plugin `source` must be a bare directory name (`"frontleap"`), not `"."` or `"./frontleap"`
- plugin files live under `plugins/<source>/`, not at the repo root

## Import in Cursor

1. **Dashboard → Plugins → Team Marketplaces → Add Marketplace → Import from Repo**
2. Use: `https://github.com/frontleapsoftware/cursor-plugin`
3. Cursor should detect **1 plugin: frontleap**
4. After merging layout changes, **Refresh** the marketplace (or re-import) so `marketplace.json` is rescanned
5. Install **frontleap**, then configure the three origin variables and connect MCP servers (Clerk OAuth)

If you see **No plugins found** / **0 plugins**, the branch you imported is still the old single-plugin layout. Use `main` after this marketplace layout is merged, then refresh.

## Plugin: frontleap

Six HTTP MCP servers (admin + client × development / QA / production). Auth is Clerk OAuth at connect time — no tokens or secrets in this repo.

| Server | URL |
| --- | --- |
| `frontleap-dev-admin` | `${FRONTLEAP_DEV_URL}/mastra/api/mcp/admin/mcp` |
| `frontleap-dev-client` | `${FRONTLEAP_DEV_URL}/mastra/api/mcp/client/mcp` |
| `frontleap-qa-admin` | `${FRONTLEAP_QA_URL}/mastra/api/mcp/admin/mcp` |
| `frontleap-qa-client` | `${FRONTLEAP_QA_URL}/mastra/api/mcp/client/mcp` |
| `frontleap-prod-admin` | `${FRONTLEAP_PROD_URL}/mastra/api/mcp/admin/mcp` |
| `frontleap-prod-client` | `${FRONTLEAP_PROD_URL}/mastra/api/mcp/client/mcp` |

### Configure

Set origins only (scheme + host, no path, no trailing slash), for example:

- `FRONTLEAP_DEV_URL` → `https://dev.example.com`
- `FRONTLEAP_QA_URL` → `https://qa.example.com`
- `FRONTLEAP_PROD_URL` → `https://app.example.com`

Do not commit real customer hostnames or secrets.

## License

Apache-2.0 — see [LICENSE](./LICENSE).
