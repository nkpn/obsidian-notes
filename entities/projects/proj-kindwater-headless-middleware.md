---
type: entity
category: project
tags: [shopify, hydrogen, oxygen, app-proxy, middleware, hmac, typescript]
project-platform: [shopify-plus]
client: kindwater
repo: kindwater-headless-middleware
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# Kindwater — Headless Hybrid Controller (Hydrogen-as-Middleware)

Creative architectural pattern: a Hydrogen storefront on [[oxygen]] is repurposed as the backend for a custom Shopify app. Net effect — zero hosting cost for app logic that would normally live on Vercel/Railway/AWS.

Internal name: `headless-controller-loyalty` v2025.1.7. Used in production for a loyalty integration.

## Stack (evidence-based, from `TECH_ANALYSIS.MD`)

| Layer | Technology |
|---|---|
| Runtime | [[oxygen]] (Cloudflare-Workers-compatible edge) |
| Framework | Remix v2 (SSR, file-based routing) |
| Shopify | `@shopify/hydrogen` 2025.1.4, `shopify-app-remix` 3.7, `shopify-api` 11.12 |
| Admin UI | Polaris v12 + App Bridge React v4 |
| Language | TypeScript 5 (strict mode, ES2022) |
| Build | Vite 6 + Shopify Hydrogen/Remix plugins |
| GraphQL | `graphql-tag` + `@graphql-codegen/cli` v5 (typed Admin API) |
| Tests | Jest 29 + ts-jest |

## The pattern

Two surfaces in one Worker:

1. **Custom Shopify App** (admin paths `/app`, `/auth`, `/webhooks`) — handles OAuth installation only.
2. **Hydrogen/Remix Worker** (everything else) — receives App Proxy traffic at `your-store.com/apps/<subpath>/*`, verifies HMAC signatures, queries Admin GraphQL, returns JSON.

### Three-context discriminated union

`app/proxyapp.server.ts` classifies every inbound request into one of three contexts (TypeScript discriminated union → compile-time safety):

| Context | Trigger | Provides |
|---|---|---|
| `public` | Path is `/` | nothing |
| `admin` | `/app`, `/auth`, `/webhooks` | full `authenticate` object |
| `proxy` | everything else | `adminClient` (GraphQL), `getCustomerId()` |

This prevents proxy routes from accidentally calling `authenticate.admin()` and admin routes from leaking adminClient into untrusted contexts.

### Signature verification (`app/lib/auth.server.ts`)

- HMAC-SHA-256 via **Web Crypto API** (`crypto.subtle`) — no Node `crypto` module since this is a Worker.
- All query params except `signature` sorted, joined, signed with `PROXY_APP_API_SECRET`.
- Constant-time comparison via `secureCompare` (timing-attack hardening).
- 401 on failure. Unit-tested with 4 cases including param-order correctness.

## Data layer

No local DB. All persistence is Shopify Admin GraphQL (`2025-04`). Customer identity comes from `?logged_in_customer_id=<id>` query injected by Shopify, promoted to `gid://shopify/Customer/<id>` for queries.

## Senior signal

- **Cost story**: a single-merchant app backend that would otherwise be ~$20/mo on Vercel runs free on Oxygen.
- **Same-origin win**: storefront calls `/apps/loyalty/...` — no CORS, no third-party cookie loss; feels first-party.
- **Trade-offs articulated honestly**:
  - `MemorySessionStorage` for OAuth — sessions don't survive Worker restarts (acceptable for single-merchant).
  - No CI/CD — deploys are CLI-only.
  - Worker CPU and no-file-I/O constraints — won't scale to heavy compute.
- **When NOT to use this pattern**: multi-merchant apps (you'd outgrow Oxygen quotas), apps that need Postgres (use a real backend), or anything CPU-heavy.

## War story shape

Storefront `fetch('/apps/loyalty/balance')` was occasionally returning stale data. Theory: caching at the Cloudflare edge. Evidence: response headers had no `cache-control`, but Workers cache by URL + querystring by default. Customer ID came in the query, but only as a sorted param — same customer = same URL = same cache. Fix: explicit `Cache-Control: private, no-store` on every proxy response. Detective method paid off: didn't reach for `kv.put`, didn't reach for an external cache, just turned off the wrong cache.

## Related

[[hydrogen]] · [[oxygen]] · [[app-proxy-pattern]] · [[shopify-plus]] · [[speroteck]]
