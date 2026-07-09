# Changelog

All notable changes to `@marocain/mcp-server` are documented here.
Format follows [Keep a Changelog](https://keepachangelog.com/).

## [0.1.8] — 2026-07-09

### Distribution (promotion — no tool/behaviour changes)
- Registry metadata for public discovery: `mcpName` in `package.json`, `server.json`
  (official MCP Registry manifest, schema 2025-12-11), `smithery.yaml`, `glama.json`.
- `publish` workflow — tag `v*` / manual dispatch publishes npm **and** registers the
  server in the official MCP Registry via GitHub OIDC.

## [0.1.7] — 2026-07-08

### Security (defense-in-depth)
- Every tool output now passes through a central `stripMoat()` at the dispatch
  layer — recursively removes the moat-bypass fields (`source_url`,
  `source_listing_id`, `source_aliases`, `agent_name`, `agency_name`) from ALL
  11 tools, not just `search_listings`/`get_listing`. The real gate is still
  server-side; this second layer means a future upstream regression can't leak
  those fields through the MCP. Idempotent — listing `enquire` pointers preserved.

## [0.1.6] — 2026-06-23

### Added (commerce — buyer & agent paid services)
- **`list_services`** — the transactable catalogue + EUR prices: buyer services (staging €9.9, viral €29, reservation €149, appraiser €299–999, visitation €399 (live API `product_type` id: `visaitation`), the €199 AI business plan, plus request-only lawyer / financing / commercialization) and agent services (listing boost €49, photo relight €19). Returns each `product_type` + `variant` id so an agent can order. Forwards to `/api/public/services`.
- **`order_service`** — place an order for any service. Instant products return a Stripe `checkout_url`; request-only products return a tracked `order_id` (no upfront charge). Forwards to `POST /api/orders`. Never returns contact; surfaces a clean `checkout_unavailable` note if card payment is temporarily offline (no raw processor error). **Eleven tools total.**

## [0.1.5] — 2026-06-23

### Added
- **`request_service`** — the moat-safe conversion tool: submit a buyer enquiry / viewing / valuation / service request for a listing. Routes through marocain's own lead flow (`POST /api/orders`, contact branch) to the listing's verified agent and returns a reference. **Never** returns agent contact. For a not-yet-claimed listing it returns a clear `listing_unclaimed` note instead of routing. **Nine tools total.**

### Security (moat hardening)
- Listing payloads (`search_listings`, `get_listing`) now **redact the origin-portal deep link** (`source_url`, `source_listing_id`, `source_aliases`) and the **agent / agency name** — closing an off-platform contact bypass a downstream agent could otherwise use to reach the seller directly. The `source` *name* (provenance) is kept; each listing gains an `enquire` pointer to `request_service`.

## [0.1.4] — 2026-06-23

### Added
- **`gin_ask`** — one-shot query to T{AI]GIN, the {GIN} agentic investment analyst (plans, searches catalogue + guides, scores with the {GIN} pillars, answers with citations). Forwards to `/api/public/gin/ask`.
- **`gin_deal_memo`** — structured, honest investor deal memo for a listing (verdict, M-Value, yield, strengths/risks, district read). Forwards to `/api/public/gin/deal-memo`.

## [0.1.3] — 2026-06-23

### Added
- New tool `semantic_search`: conceptual / vector search across the catalogue **and** the authored guides (Foreign Buyer's Playbook, Morocco-vs-Dubai thesis, AI-scoring methodology, residency, city theses). Hybrid retrieval (vector + full-text RRF). Forwards to `/api/public/semantic-search`. Moat-safe — never returns agent contact details.

## [0.1.2] — 2026-06-16

### Quality
- Extracted pure helpers (`resolveBase`, `qs`) into `lib.js`.
- Added unit tests via the Node 18+ built-in test runner — **zero new dev dependencies**. `npm test`.
- GitHub Actions CI matrix (Node 18 / 20 / 22): unit tests + black-box MCP handshake smoke (`test/smoke.mjs`) on every push/PR.
- README: npm + CI + license + Node badges; response-shape examples for `search_listings` + `get_gin_score`; troubleshooting table.

### No behaviour change
- Tool surface, schemas, and runtime behaviour are identical to 0.1.1.

## [0.1.1] — 2026-06-16

### Security & robustness
- **SSRF guard** on `MAROCAIN_API_BASE`: an override base must be HTTPS and an
  allow-listed public host (`marocain.investments`), otherwise it falls back to
  the canonical origin. Blocks pivots to localhost / private IPs / metadata.
- **Request timeout** (30s, `MAROCAIN_TIMEOUT_MS`) via `AbortController` — a hung
  upstream no longer blocks the server indefinitely.
- **Input bounds** on tool arguments (`maxLength` on text, `minimum`/`maximum` on
  numbers, capped `limit`).
- Upstream errors return a generic user-facing message; internal status/path is
  logged to stderr only.
- Pinned `@modelcontextprotocol/sdk` to `~1.29.0` (patch-only) for reproducible installs.

## [0.1.0] — 2026-06-16

### Added
- Initial release. Stdio MCP server with 5 moat-safe {GIN} tools
  (`search_listings`, `get_listing`, `get_gin_score`, `get_market`,
  `listing_derive`) forwarding to `https://marocain.investments/api/public/*`.
