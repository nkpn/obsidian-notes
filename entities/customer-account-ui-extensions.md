---
type: entity
category: api
tags: [shopify, ui-extensions, customer-account, preact]
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# Customer Account UI Extensions

Shopify's extension surface for injecting custom UI into the post-purchase customer account area (orders, profile, returns, tracking). Replaces what previously lived in custom theme pages or third-party apps.

## Architecture

- Author UI in **Preact** (Shopify ships an opinionated `@shopify/ui-extensions/preact` runtime).
- Composed from semantic web-component-style primitives: `<s-section>`, `<s-stack>`, `<s-box>`, `<s-text>`, `<s-link>`, etc.
- Each extension declares **targets** (where Shopify injects it) in `shopify.extension.toml`.
- Data fetched through the Customer Account GraphQL API at `shopify:customer-account/api/<version>/graphql.json` — auth handled by Shopify, no token plumbing.

## Common targets (2025-10 / 2026-01 APIs)

- `customer-account.footer.render-after` — footer-level help/info blocks.
- `customer-account.order.action.menu-item.render` — "Start a return", "Track" actions on the order detail.
- `customer-account.profile.block.render` — profile preferences block.
- `customer-account.order-status.cart-line-list.render-after` — order status / thank-you screen extras.

## Senior signal

- These extensions are the official replacement path for legacy customer-account customizations and for `checkout.liquid` post-purchase logic.
- Constraints: no DOM access outside the iframe, GraphQL only, i18n via `shopify.i18n.translate(key)` — don't reach for vanilla DOM tricks.
- For multi-region stores, branching by `metafield(namespace, key)` (e.g., a Global-E order ID) is the pattern for cross-platform return/tracking flows ([[proj-cole-haan-customer-account-ui]]).

## Used in

[[proj-cole-haan-customer-account-ui]] · [[proj-checkout-extensions-gift-message]]

## Related

[[shopify-plus]] · [[checkout-ui-extensions]] · [[liquid]]
