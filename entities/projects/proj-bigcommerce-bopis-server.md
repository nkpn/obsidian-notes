---
type: entity
category: project
tags: [bigcommerce, bopis, express, fly-io, omnichannel]
project-platform: [bigcommerce-catalyst]
repo: bigcommerce-server-BOPIS-fly-io
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# BigCommerce BOPIS Server (Fly.io)

Backend half of the [[bopis]] (Buy Online, Pick Up In Store) flow for BigCommerce stores. Node.js + Express + `node-bigcommerce`, deployed to [[fly-io]] at `bigcommerce-server-bopis-fly-io.fly.dev`. **Matches the "BOPIS platform on Node.js/Express + Fly.io for real-time omnichannel fulfilment" CV claim** ([[2026-05-05-02-cv-merge-speroteck]]).

## Stack (evidence)

- `express` 4.18, `cors`, `dotenv`.
- `node-bigcommerce` 4.1 — BigCommerce v3 API client.
- Single `server.js` (intentionally simple — one file, one purpose).
- Dockerfile + `fly.toml`. Node 18+.

## API surface

| Method | Path | Backed by | Purpose |
|---|---|---|---|
| GET | `/api/products` | `/catalog/products?include=images` | Products with optional `category_id` and `inventory_level` filters. |
| GET | `/api/inventory/items` | `/inventory/items?variant_id:in=...` | Variant-level inventory. |
| GET | `/api/inventory/items/id` | `/inventory/items?product_id:in=...` | Product-level inventory. |
| GET | `/api/pickup/methods` | `/pickup/methods?id:in=...` | Pickup methods for given location IDs. |
| POST | `/api/checkout/:checkoutId/consignments` | `/checkouts/{id}/consignments?include=consignments.available_shipping_options,consignments.available_pickup_options` | Add consignment to in-flight checkout (the moment the buyer picks "pickup"). |
| PUT | `/api/checkout/:checkoutId/consignments/:consignmentId` | `/checkouts/{id}/consignments/{cid}` | Update consignment (e.g., switch pickup location). |

The `?include=` on the consignment POST is the critical detail — same call returns updated shipping + pickup options so the storefront doesn't need a follow-up request to refresh the UI.

## Why an Express server (and not Stencil JS)

- BigCommerce's storefront-side APIs don't expose location-scoped inventory the way the v3 server-to-server `/inventory/items` endpoint does.
- The `X-Auth-Token` would have to be embedded in the theme JS otherwise — an instant credential leak.
- Stencil-side fetches go through the storefront origin; a separate API service avoids cookie/session contamination from the cart flow.

## Why Fly.io

- Always-on small machine — no cold starts during a checkout session (cold starts during BOPIS = abandoned carts).
- Simple `fly secrets set` for `BIGCOMMERCE_CLIENT_ID`, `BIGCOMMERCE_ACCESS_TOKEN`, `BIGCOMMERCE_STORE_HASH`.
- One region pin keeps the egress IP stable in case BigCommerce ever rate-limits per source.

## Pair with

[[proj-board-game-barrister-bopis]] — the Stencil theme that calls this server.

## Senior signal

- **"Why expose `/api/checkout/:id/consignments` from your own server instead of letting the storefront call BigCommerce directly?"** → Storefront API can't authenticate as the merchant; v3 endpoints require X-Auth-Token; that token cannot ship to the browser. The proxy server is the only correct shape.
- **"What's the failure mode you have to handle?"** → Stale inventory between read and reserve. Real BOPIS systems compensate by re-checking at consignment-add time and surfacing "this item just sold out" inline, not at the payment step.

## Related

[[bopis]] · [[bigcommerce-catalyst]] · [[fly-io]] · [[proj-board-game-barrister-bopis]] · [[speroteck]]
