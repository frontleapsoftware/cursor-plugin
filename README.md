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
plugins/frontleap-dev-admin/
  .cursor-plugin/plugin.json
  mcp.json
plugins/frontleap-dev-client/
  .cursor-plugin/plugin.json
  mcp.json
plugins/frontleap-qa-admin/
  .cursor-plugin/plugin.json
  mcp.json
plugins/frontleap-qa-client/
  .cursor-plugin/plugin.json
  mcp.json
LICENSE
README.md
```

Rules that matter for discovery:

- `metadata.pluginRoot` must be `"plugins"`
- each plugin `source` must be a bare directory name (for example `"frontleap-admin"`), not `"."` or `"./frontleap-admin"`
- plugin files live under `plugins/<source>/`, not at the repo root

## Import in Cursor

1. **Dashboard → Plugins → Team Marketplaces → Add Marketplace → Import from Repo**
2. Use: `https://github.com/frontleapsoftware/cursor-plugin`
3. Cursor should detect **6 plugins** (admin + client × production / development / QA)
4. After merging marketplace changes, **Refresh** the marketplace (or re-import) so `marketplace.json` is rescanned
5. Install the plugins for the environments you need, set one origin per plugin, then connect (Clerk OAuth)

If you previously installed an older multi-environment Frontleap plugin, uninstall it and install the environment-specific admin and/or client plugins instead.

## Which plugin to install

| Plugin | Environment | Install when you need… |
| --- | --- | --- |
| **frontleap-admin** | Production | Platform Admin MCP |
| **frontleap-client** | Production | Task Configuration / client MCP |
| **frontleap-dev-admin** | Development | Platform Admin MCP |
| **frontleap-dev-client** | Development | Task Configuration / client MCP |
| **frontleap-qa-admin** | QA | Platform Admin MCP |
| **frontleap-qa-client** | QA | Task Configuration / client MCP |

You can install any combination. Each plugin asks for a single `FRONTLEAP_URL` origin for that environment. The existing `frontleap-admin` and `frontleap-client` plugins are the production pair; leave them unchanged and use the `-dev-` / `-qa-` plugins for other environments.

## Plugin: frontleap-admin (production)

One HTTP MCP server. Auth is Clerk OAuth at connect time — no tokens or secrets in this repo.

| Server | URL |
| --- | --- |
| `frontleap-admin` | `${FRONTLEAP_URL}/mastra/api/mcp/admin/mcp` |

## Plugin: frontleap-client (production)

One HTTP MCP server. Auth is Clerk OAuth at connect time — no tokens or secrets in this repo.

| Server | URL |
| --- | --- |
| `frontleap-client` | `${FRONTLEAP_URL}/mastra/api/mcp/client/mcp` |

## Plugin: frontleap-dev-admin

| Server | URL |
| --- | --- |
| `frontleap-dev-admin` | `${FRONTLEAP_URL}/mastra/api/mcp/admin/mcp` |

## Plugin: frontleap-dev-client

| Server | URL |
| --- | --- |
| `frontleap-dev-client` | `${FRONTLEAP_URL}/mastra/api/mcp/client/mcp` |

## Plugin: frontleap-qa-admin

| Server | URL |
| --- | --- |
| `frontleap-qa-admin` | `${FRONTLEAP_URL}/mastra/api/mcp/admin/mcp` |

## Plugin: frontleap-qa-client

| Server | URL |
| --- | --- |
| `frontleap-qa-client` | `${FRONTLEAP_URL}/mastra/api/mcp/client/mcp` |

### Configure

Set one origin per installed plugin (scheme + host only, no path, no trailing slash), for example:

- `FRONTLEAP_URL` → `https://slug.frontleap.com`

Point each plugin at the matching environment origin. Do not commit real customer hostnames or secrets.

## License

Apache-2.0 — see [LICENSE](./LICENSE).
