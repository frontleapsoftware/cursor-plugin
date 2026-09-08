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
5. Install either or both plugins, set one origin for the environment you are using, then connect (Clerk OAuth)

If you previously installed an older multi-environment Frontleap plugin, uninstall it and install **frontleap-admin** and/or **frontleap-client** instead.

## Which plugin to install

| Plugin | Install when you need… |
| --- | --- |
| **frontleap-admin** | Platform Admin MCP |
| **frontleap-client** | Task Configuration / client MCP |

You can install one or both. Each plugin asks for a single `FRONTLEAP_URL` origin for the environment you are targeting.

## Plugin: frontleap-admin

One HTTP MCP server. Auth is Clerk OAuth at connect time — no tokens or secrets in this repo.

| Server | URL |
| --- | --- |
| `frontleap-admin` | `${FRONTLEAP_URL}/mastra/api/mcp/admin/mcp` |

## Plugin: frontleap-client

One HTTP MCP server. Auth is Clerk OAuth at connect time — no tokens or secrets in this repo.

| Server | URL |
| --- | --- |
| `frontleap-client` | `${FRONTLEAP_URL}/mastra/api/mcp/client/mcp` |

### Configure

Set one origin per installed plugin (scheme + host only, no path, no trailing slash), for example:

- `FRONTLEAP_URL` → `https://your-frontleap-origin.example`

Point it at the environment you are using (development, QA, or production). Do not commit real customer hostnames or secrets.

## License

Apache-2.0 — see [LICENSE](./LICENSE).
