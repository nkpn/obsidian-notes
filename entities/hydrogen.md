---
type: entity
category: framework
tags: [shopify, react, headless, frontend]
mentioned-in: [[2026-05-05-03-shopify-interview-prep]]
---

# Hydrogen

Shopify's React-based framework for headless storefronts. Pairs with Oxygen (Shopify's edge hosting).

## 2026 state

- Now on **React Router v7** (the post-Remix-merge SPA/SSR framework).
- Tightly integrated with the Storefront API and Customer Account API.

## Senior signal

When *not* to use Hydrogen:
- Small catalog, tight timeline → Online Store 2.0 themes are faster to ship.
- Marketing-heavy site that lives on conversion polish, not differentiation → less surface area in themes.
- Team without React experience → maintenance cost outweighs flexibility.

When Hydrogen wins:
- Multi-source data (e.g., Shopify + headless CMS + ERP).
- Complex UX that themes can't express.
- Performance budget that needs edge SSR (Oxygen).

## Related

[[shopify-plus]] · [[liquid]]
