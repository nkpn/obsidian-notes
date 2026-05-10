---
type: entity
category: project
tags: [shopify, checkout-ui-extensions, customer-account-ui-extensions, metafields]
project-platform: [shopify-plus]
repo: shopify-app-test/checkout-extensions
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# Order Gift Message — Checkout + Account Extensions

Two-extension Shopify app that lets a buyer add a gift message at checkout and re-displays it on the customer account order-status view. Demonstrates the round-trip between [[checkout-ui-extensions]] and [[customer-account-ui-extensions]] via app-scoped metafields.

Author: nkpn. Workspace: `extensions/*`. API version `2026-01` (latest at build time).

## Extensions

### `order-gift-message` (checkout)

- Target: `purchase.checkout.block.render`.
- Source: `src/Checkout.jsx`.
- Settings field: `giftMessage` (`multi_line_text_field`) — merchant-configurable label.
- Writes to a metafield: `[[extensions.metafields]] namespace = "app", key = "giftmessage"`.
- `api_access = true` — uses the Storefront API to attach the message to the order.

### `account-order-gift-message` (customer account)

- Target: `customer-account.order-status.cart-line-list.render-after`.
- Source: `src/OrderStatusBlock.jsx`.
- Reads the same metafield on render: `[[extensions.targeting.metafields]] namespace = "$app", key = "giftmessage"`.

## Why this is interview-shaped

The whole flow is a microcosm of the modern Shopify customization story:

1. **Capture in checkout** — UI extension, not `checkout.liquid`.
2. **Persist as a metafield** — declared in the extension toml, not via app-side mutation.
3. **Surface in customer account** — a different UI extension reading the same metafield.

No app server needed for the data path. The whole thing is declarative + JSX render.

## Senior signal

- **`namespace = "$app"` vs `namespace = "app"`**: `$app` is a reserved namespace that Shopify auto-scopes to your app's UID; it's the right choice when you don't want metafield collisions across apps. Using bare `"app"` (as the checkout side does) is the legacy form — interviewers test which you reach for.
- **API version 2026-01**: knowing the upgrade notes from 2026-04 (which broke a few extension targets) signals you watch the changelog.
- **When NOT to use this pattern**: anything stateful or that needs cross-customer aggregation → app-side DB, not metafields.

## Related

[[checkout-ui-extensions]] · [[customer-account-ui-extensions]] · [[shopify-plus]]
