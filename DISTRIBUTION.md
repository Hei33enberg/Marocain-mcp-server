# Distribution / promotion — where `@marocain/mcp-server` is (or should be) listed

Goal: make the already-built FOP L2 node + MCP server **discoverable by AI agents**
wherever they look for tools. All the analysis is free and moat-safe, so listing is
pure upside (agents find marocain → cite it → drive traffic).

Status legend: ✅ done in-repo · 🔵 one owner action · ⚪ optional.

## 1. npm — ✅ published (`@marocain/mcp-server`, latest 0.1.7 → 0.1.8 on next publish)
Bumped to **0.1.8** with the `mcpName` field the MCP Registry requires. Publishing
is wired into `.github/workflows/publish.yml`.

## 2. Official MCP Registry (registry.modelcontextprotocol.io) — 🔵 one click
The canonical meta-registry many clients read. Everything is prepared:
- `server.json` (schema 2025-12-11, name `io.github.hei33enberg/marocain-mcp-server`).
- `package.json` carries the matching `mcpName`.
- `.github/workflows/publish.yml` publishes npm **and** registers the server via
  **GitHub OIDC** (no registry token needed).

**To publish (owner):**
1. Confirm the repo secret **`NPM_TOKEN`** exists (Settings → Secrets → Actions). If
   not, create an npm automation token and add it.
2. Either push a tag — `git tag v0.1.8 && git push origin v0.1.8` — **or** open the
   Actions tab → "publish" → **Run workflow**.
3. Verify: `curl "https://registry.modelcontextprotocol.io/v0.1/servers?search=marocain"`.

> Alternative (no CI): install the CLI locally, `mcp-publisher login github` (device
> code), `mcp-publisher publish` from the repo root.

## 3. Smithery (smithery.ai) — 🔵 connect repo
`smithery.yaml` (classic stdio, `npx -y @marocain/mcp-server`, no config/secrets) is
committed. To list: go to **smithery.ai → Add Server → connect the GitHub repo
`Hei33enberg/marocain-mcp-server`** and authorise. The README already carries the
Smithery badge (lights up once listed).

## 4. Glama (glama.ai) — ✅ auto-indexes / ⚪ claim
Glama crawls public MCP repos automatically; `glama.json` (maintainers) is committed
and the README carries the Glama badge. Optionally sign in at glama.ai to **claim**
the server and confirm metadata.

## 5. mcp.so — ⚪ submit
Community directory. Submit at **mcp.so/submit** with:
- Name: `marocain.investments — {GIN} real-estate intelligence`
- npm: `@marocain/mcp-server` · Repo: `github.com/Hei33enberg/marocain-mcp-server`
- Tools: search_listings, get_listing, get_gin_score, get_market, listing_derive, semantic_search
- One-liner: "AI-graded Moroccan luxury real estate — honest {GIN} verdict, M-Value AVM, market facts. Moat-safe."

## 6. modelcontextprotocol/servers community list — ⚪ PR
Add a line to the "Community Servers" section of
`github.com/modelcontextprotocol/servers` (alphabetical). Suggested entry:
```
- **[marocain.investments](https://github.com/Hei33enberg/marocain-mcp-server)** – {GIN} real-estate intelligence for Morocco: scoring verdict, M-Value AVM, market facts, semantic search. Moat-safe.
```

## 7. On-site advertising — ✅ live
Already advertised from the domain (AI crawlers + agents read these):
- `/.well-known/ai-plugin.json` (MCP block, all 6 tools) · `/.well-known/fop.json`
- `/llms.txt` (npx install + tool list) · `/api/public/skills.json` · `/api/public/openapi.json`
- `robots.txt` AI-bot stanzas (GPTBot / ClaudeBot / PerplexityBot).

## 8. Claude Connectors directory — ⚪ owner
Anthropic's connector directory is invite/submission based; once the server is in the
official MCP Registry (#2) it's eligible. Submit via the Anthropic connectors form.
