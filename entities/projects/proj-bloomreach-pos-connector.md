---
type: entity
category: project
tags: [shopify, shopify-app, pos, react-router, bloomreach, prisma, polaris]
project-platform: [shopify-plus]
repo: bloomreach-pos
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# Bloomreach POS Connector

Shopify App that connects Shopify POS to **Bloomreach** (customer engagement / CDP / analytics platform). Two extensions: a POS UI tile that surfaces customer insights for the cashier, and a checkout UI extension for coupon validation.

Internal name: `pos-bloomreach-connector`. Author: provo (Speroteck).

## Stack (evidence-based)

- **Framework**: `@shopify/shopify-app-react-router` v1 (the new RR-based template, post-Remix).
- **React Router v7** `@react-router/dev`, `@react-router/serve` — full SSR app.
- **Prisma** v6 with `@shopify/shopify-app-session-storage-prisma` v7 (durable session storage, not memory).
- **Polaris** types + App Bridge React v4 for the embedded admin UI.
- **GraphQL codegen** for typed Admin API queries (`@shopify/api-codegen-preset`).
- Node 20.19+ / 22.12+, ES modules, Vite 6, Dockerfile present (`docker-start` runs `prisma migrate deploy && start`).

## Extensions

### `bloomreach-pos-visit-tracker` (POS UI extension)

- API version `2025-10`, type `ui_extension`, Preact-based.
- Targets: `pos.home.tile.render` (home screen tile) and `pos.home.modal.render` (modal triggered from the tile).
- Surfaces Bloomreach customer insights to the in-store cashier.

### `coupons-validation` (Checkout UI extension)

- API version `2025-10`, target `purchase.checkout.block.render`.
- `api_access = true` — direct Storefront API access for cart/discount inspection.
- Validates coupons against Bloomreach's loyalty/segmentation rules at checkout.

## Backend routes (evidence)

Routes under `app/routes/` that do real work:

- `api.pos.bloomreach_visit.tsx` — POST endpoint for the POS extension to record a visit. Authenticates via `authenticate.pos(request)` + `authenticate.admin(request)`, maps a `PosVisitEventPayload` through `BloomreachEventMapper`, sends through `BloomreachClient`.
- `api.pos.customer_insights.tsx` — pulls customer profile from Bloomreach to display on the tile.
- `api.pos.get_customer_email.tsx` / `api.pos.update_consent.tsx` — consent management.

`app/lib/bloomreach/` holds: `BloomreachConfig`, `BloomreachClient`, `BloomreachEventMapper`. Clean separation between integration config, transport, and event mapping.

## Senior signal

- **POS authentication is its own scope**: `authenticate.pos(request)` is distinct from `authenticate.admin(request)`. The route has to call **both** — POS for the request signature, admin for the GraphQL session. Easy to miss.
- **Per-shop env**: `getBloomreachEnvForShop(shop)` keeps Bloomreach project tokens scoped per merchant — no global creds.
- **CORS**: `corsHeaders(request)` + explicit `OPTIONS` handler. POS UI extensions run in a constrained webview that hits CORS preflight; senior devs handle it explicitly, mid-level devs `*` it.
- **Why React Router v7 instead of Remix?**: Shopify's app template moved off Remix when Remix → React Router merge happened. Using the new template signals you're tracking the platform.

## War story shape

Cashier scanned a customer at POS; tile showed insights but consent updates silently failed. Theory: the POST was hitting the route but auth was dropping. Evidence: log inspection showed `authenticate.pos(request)` succeeded but `authenticate.admin(request)` returned no session for the shop — admin session had expired and was rebuilt mid-flow. Fix: order matters — POS auth first (cheap, request-signature), then admin auth (which will refresh if needed).

## Related

[[checkout-ui-extensions]] · [[shopify-plus]] · [[speroteck]]
