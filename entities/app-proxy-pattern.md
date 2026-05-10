---
type: entity
category: pattern
tags: [shopify, app, proxy, hmac]
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# App Proxy Pattern

Shopify's App Proxy lets a Custom App expose backend routes under the merchant's own domain at `your-store.com/apps/<subpath>/*`. Shopify forwards requests to the proxy URL configured in the app, signing them with an HMAC of all query params.

## Verification (the part that gets faked in mid-level interviews)

- HMAC-SHA-256 over **sorted query params** (excluding `signature`), keyed by the app's API secret.
- Constant-time compare via `secureCompare` (avoid timing attacks).
- 401 on failure — no fallback path. Fail fast.

## What Shopify injects automatically

- `?logged_in_customer_id=<id>` — promoted to `gid://shopify/Customer/<id>` for Admin API calls.
- `?shop=<store>.myshopify.com` — for multi-tenant routing.

## When to reach for it

- Storefront needs custom data the Storefront API can't provide cheaply (tier-pricing logic, loyalty balance, multi-source aggregation).
- A custom JS widget on the theme calls `fetch('/apps/<subpath>/api/...')` — feels first-party because the host matches the merchant domain.

## Three-context discriminated union (kindwater pattern)

Implemented in [[proj-kindwater-headless-middleware]]: every incoming request is classified as `public`, `admin`, or `proxy` and the handler gets a typed dependency bag matched to that context. Prevents proxy routes from accidentally calling `authenticate.admin()` and vice versa. Compile-time safe.

## Senior signal

A mid-level dev hosts the app backend on Vercel and shrugs at $20/mo. A senior dev points out that for **single-merchant** apps, [[oxygen]] gets you the backend for free, the proxy gives you same-origin requests (no CORS), and HMAC verification is 30 lines of Web Crypto.

## Used in

[[proj-kindwater-headless-middleware]]

## Related

[[oxygen]] · [[hydrogen]] · [[shopify-plus]]
