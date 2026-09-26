# Frontleap Team Marketplace (Cursor + Claude Code)

This repository is a **dual marketplace**: the same twelve plugins work in **Cursor** and **Claude Code**. It is not a single-root plugin repo.

Team marketplaces require this layout:

```text
.cursor-plugin/marketplace.json   # Cursor Team Marketplace
.claude-plugin/marketplace.json   # Claude Code marketplace
plugins/<plugin-name>/
  .cursor-plugin/plugin.json
  .claude-plugin/plugin.json
  .mcp.json
LICENSE
README.md
```

Rules that matter for discovery:

- Cursor: `metadata.pluginRoot` must be `"plugins"`; each plugin `source` is a bare directory name (for example `"frontleap-admin"`)
- Claude Code: `metadata.pluginRoot` is `"./plugins"`; bare `source` names resolve under that root
- Each plugin ships **one** shared MCP file: `.mcp.json` (Claude’s default). Cursor pins `"mcpServers": "./.mcp.json"` so both clients use the same hosted HTTP + OAuth server
- Do not add a sibling `mcp.json` with a different payload — dual Cursor + Claude repos that ship both filenames can load the wrong server
- Origins are **hardcoded** per plugin (no `FRONTLEAP_URL` install variable)

## Import in Cursor

1. **Dashboard → Plugins → Team Marketplaces → Add Marketplace → Import from Repo**
2. Use: `https://github.com/frontleapsoftware/cursor-plugin`
3. Cursor should detect **12 plugins** (admin + client × Demo / Frontleap / Stella Jones / Canac Dev / Canac Non-Prod / Canac)
4. After merging marketplace changes, **Refresh** the marketplace (or re-import) so `marketplace.json` is rescanned
5. Install the plugins for the environments you need, then connect (Clerk OAuth)

If you previously installed plugins that asked for `FRONTLEAP_URL`, uninstall them and install the origin-specific plugins instead.

## Install in Claude Code

1. Add the marketplace:
   ```shell
   /plugin marketplace add frontleapsoftware/cursor-plugin
   ```
2. Install the plugins you need (suffix is the marketplace name `frontleap`), for example:
   ```shell
   /plugin install frontleap-admin@frontleap
   /plugin install frontleap-client@frontleap
   /plugin install frontleap-canac-admin@frontleap
   /plugin install frontleap-canac-client@frontleap
   ```
3. Run `/reload-plugins`, then authenticate via Clerk OAuth when Claude Code connects to the MCP server (`/mcp`)

## Which plugin to install

| Plugin | Origin |
| --- | --- |
| **frontleap-demo-admin** / **frontleap-demo-client** | `https://demo.internal.frontleap.com` |
| **frontleap-admin** / **frontleap-client** | `https://frontleap.internal.frontleap.com` |
| **frontleap-stella-jones-admin** / **frontleap-stella-jones-client** | `https://stella-jones.internal.frontleap.com` |
| **frontleap-canac-dev-admin** / **frontleap-canac-dev-client** | `https://canac.dev.frontleap.com` |
| **frontleap-canac-non-prod-admin** / **frontleap-canac-non-prod-client** | `https://canac.non-prod.frontleap.com` |
| **frontleap-canac-admin** / **frontleap-canac-client** | `https://canac.frontleap.com` |

You can install any combination. Each plugin points at a fixed origin and appends the admin or client MCP path.

## MCP paths

Auth is Clerk OAuth at connect time — no tokens or secrets in this repo.

| Role | Path |
| --- | --- |
| Admin | `/mastra/api/mcp/admin/mcp` |
| Client | `/mastra/api/mcp/client/mcp` |

Example full URL for Canac admin: `https://canac.frontleap.com/mastra/api/mcp/admin/mcp`

## License

Apache-2.0 — see [LICENSE](./LICENSE).
