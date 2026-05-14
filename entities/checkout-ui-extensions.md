---
type: entity
category: api
tags: [shopify, ui-extensions, checkout]
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]], [[2026-05-14-02-interview-self-introduction-prep]]
---

# Checkout UI Extensions

Shopify's extension surface for injecting UI into the checkout flow. The migration target for `checkout.liquid` (Plus stores must move by 2026-04 release; non-Plus checkout scripts off Aug 26, 2026 — see [[shopify-plus]]).

## Common targets

- `purchase.checkout.block.render` — generic block injection in checkout (custom fields, gift messages, validators).
- `purchase.checkout.delivery-address.render-before/after` — address-step injections.
- `purchase.checkout.cart-line-list.render-after` — cart summary additions.

## Capabilities flags (`shopify.extension.toml`)

- `api_access = true` — direct Storefront/Admin API queries from inside the extension.
- `network_access = true` — outbound `fetch()` to third-party endpoints (e.g., for coupon validation against an external loyalty system).
- `block_progress = true` — extension can prevent buyer from advancing (validators only).

## Settings + metafields

Extensions can declare `[[extensions.settings.fields]]` for merchant-configured strings and `[[extensions.metafields]]` to read from app-scoped (`namespace = "$app"`) or store-scoped metafields. Used for binding gift messages, loyalty IDs, etc., back to the order ([[proj-checkout-extensions-gift-message]]).

## Senior signal

- 2026-04 brought breaking changes to the checkout UI extensions API — knowing the upgrade notes is a senior tell.
- When NOT to reach for a checkout UI extension: backend logic that needs to mutate cart/discount/shipping at decision time → use [[shopify-functions]] instead. UI extensions render; Functions decide.

## Used in

[[proj-checkout-extensions-gift-message]] · [[proj-bloomreach-pos-connector]] (coupon validation)

## Related

[[shopify-plus]] · [[shopify-functions]] · [[customer-account-ui-extensions]]
