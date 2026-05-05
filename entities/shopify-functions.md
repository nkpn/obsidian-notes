---
type: entity
category: api
tags: [shopify, backend, rust, wasm]
mentioned-in: [[2026-05-05-03-shopify-interview-prep]]
---

# Shopify Functions

Custom backend logic that runs inside Shopify's commerce flow — discounts, delivery customizations, payment customizations, cart transforms, validations. Compiled to **Wasm** (typically authored in Rust or JS).

## Why they exist

Replace [[liquid]] checkout customizations and Shopify Scripts with sandboxed, fast, portable logic. This is the migration target for the Scripts deprecation (June 30, 2026).

## Constraints (interview-relevant)

- Must run in <5 ms typically — no I/O, no external HTTP.
- Stateless — input → output, no side effects allowed inside the function.
- For data not in the function input, use `network_access` configurations or hand off to an app webhook.

## Senior signal

When *not* to reach for Functions:
- Logic that needs external API calls → app + webhook is the right shape.
- Logic UI-driven at checkout → checkout UI extensions instead.
- Anything stateful → app + database.

## Related

[[shopify-plus]]
