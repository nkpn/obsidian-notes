---
type: entity
category: platform
tags: [shopify, edge, cloudflare-workers, hosting]
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]], [[2026-05-14-02-interview-self-introduction-prep]]
---

# Oxygen

Shopify's free, Cloudflare-Workers-compatible edge runtime that hosts [[hydrogen]] storefronts. Deployed via `shopify hydrogen deploy`. No servers, no Dockerfile, no CI/CD plumbing — it's CLI-driven.

## Constraints

- Workers runtime: no native Node `fs`/`net`, must use Web Crypto / Web APIs.
- CPU time per request limited (typical Cloudflare-style budget).
- No persistent local state — durable storage is via Storefront/Admin GraphQL or external KV.
- One built artifact (`dist/server/index.js`) per deploy.

## The non-obvious play

[[proj-kindwater-headless-middleware]] uses Oxygen **not as a storefront** but as a free middleware host for an [[app-proxy-pattern]] backend. The pattern:

1. Custom Shopify App owns OAuth + App Proxy config.
2. Hydrogen worker on Oxygen receives proxied requests at `your-store.com/apps/<subpath>/*`.
3. Worker verifies HMAC signature, queries Admin GraphQL, returns JSON to the storefront.

Result: zero hosting cost for a single-merchant app backend.

## Senior signal

Knowing Oxygen exists as a generic edge target (not just a storefront host) and being able to articulate the constraint trade-offs (no DB, CPU limits) vs. the cost win is the differentiator.

## Related

[[hydrogen]] · [[shopify-plus]] · [[app-proxy-pattern]]
