---
type: entity
category: project
tags: [shopify, shopify-plus, customer-account, preact, enterprise, global-e]
project-platform: [shopify-plus]
client: cole-haan
repo: cole-haan-extentions
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# Cole Haan — Customer Account UI Extensions

Enterprise-tier Shopify Plus project for Cole Haan (US footwear/leather brand). Extension-only Shopify app — no embedded admin, no server. Pure UI extensions in the customer account surface, deployed via Shopify CLI.

Internal name: `ch-account-extentions` (npm workspace, `extensions/*`).

## Stack (evidence-based)

- Shopify CLI extension-only template (no `app/` folder, no Polaris).
- Preact + `@shopify/ui-extensions/preact` runtime.
- API version `2025-10`.
- GraphQL via `shopify:customer-account/api/<version>/graphql.json` (no fetch tokens — Shopify-managed).

## Extension surface

One `customer-account-ui` extension with **four targets**:

| Target | File | Purpose |
|---|---|---|
| `customer-account.footer.render-after` | `CustomerCareAppBlock.jsx` | Help/contact card block in the account footer, i18n-driven. |
| `customer-account.order.action.menu-item.render` | `OrderMenuItem.jsx` | "Start a return" action on order detail — branches between domestic and Global-E orders. |
| `customer-account.profile.block.render` (placement: `PROFILE1`) | `CustomerProfilePreferences.jsx` | Profile preferences block. |
| (additional) | `OrderStatus.jsx`, `TrackingButton.jsx` | Order status and tracking link rendering. |

## Integrations / customizations worth talking about

- **Global-E (cross-border commerce)**: orders that originated through Global-E carry a `metafield(namespace: "colehaan", key: "ge_order_id")`. The return action branches:
  - With `ge_order_id` → returns flow opens `https://web.global-e.com/Returns/FindOrder?OrderID=...&ShippingEmail=...&ShowLayout=true`.
  - Without → standard `${shopUrl}/apps/returns?order=...&email=...` flow.
- **Tracking**: pulls `fulfillments.trackingInformation` plus the same Global-E metafield to construct a Bglobal-E tracking URL when applicable.
- **i18n**: every visible string goes through `shopify.i18n.translate(key)` — no hardcoded copy.

## Senior signal (interview talking points)

- "Why an extension-only app instead of a custom theme page?" → Customer account is moving off Liquid templates entirely; the extension surface is the official path forward and survives theme swaps.
- "How do you handle multi-region returns?" → Branch by metafield, not by customer locale — orders carry their origin platform in metafields, locale lies.
- "Why Preact and not React?" → It's not a choice — Shopify ships Preact in `@shopify/ui-extensions/preact`; the bundle size budget per extension is tight.
- "What's the auth story?" → Customer Account GraphQL is auth-handled by Shopify; the extension never sees a token. This is intentional and removes a whole class of leak.

## War story shape

Returns flow had to behave differently for orders shipped via Global-E vs. domestic, but the extension can't ask "where was this order placed". Theory: there must be a marker on the order. Evidence: found `colehaan.ge_order_id` metafield set on Global-E imports. Fix: branch the menu item URL on metafield presence — surgical, ~10 lines, no extra round-trip.

## Related

[[customer-account-ui-extensions]] · [[shopify-plus]] · [[liquid]] · [[speroteck]]
