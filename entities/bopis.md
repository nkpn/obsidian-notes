---
type: entity
category: concept
tags: [ecommerce, omnichannel, fulfillment, bigcommerce]
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# BOPIS — Buy Online, Pick Up In Store

Omnichannel fulfilment pattern: customer orders online, picks up at a physical store. Real-time location-aware inventory + a checkout flow that swaps shipping for an in-store consignment.

## What the platform doesn't give you out of the box

- **Cross-location inventory in cart**: standard cart only knows site-wide stock; BOPIS needs per-location availability for every line.
- **Geolocation-ranked store list**: pick-up UI must filter and sort by proximity.
- **Consignment swap at checkout**: changing a checkout from "ship" to "pickup" mutates consignments, available shipping options, and tax — has to be POST/PUT against the platform's checkout API mid-session.

## Implementation surfaces

In Nikita's stack the BOPIS flow split cleanly across two repos:

- **Backend** ([[proj-bigcommerce-bopis-server]]): Node.js/Express on [[fly-io]] talking to BigCommerce v3 (`/catalog/products`, `/inventory/items`, `/pickup/methods`, `/checkouts/{id}/consignments`). Hosted on Fly because the API call volume is concentrated and the cold-start budget is real.
- **Frontend** ([[proj-board-game-barrister-bopis]]): BigCommerce Stencil theme that hooks into cart and checkout pages. Polls inventory, runs IP-based geolocation to seed the store list, hides shipping when no pickup-eligible locations exist for the cart contents.

The two pieces talk over a public Fly URL; the storefront calls it from `cart.js` / checkout JS.

## Senior signal

- Articulating the **failure modes**: stale inventory between read and reserve, race when two customers grab the last unit, geolocation-permission denial defaulting to IP fallback.
- Knowing why BOPIS lives in a separate Express service instead of Stencil: theme JS can't hold long-lived API tokens and BigCommerce's storefront API doesn't expose location-scoped inventory the way the v3 server-to-server API does.

## Related

[[proj-bigcommerce-bopis-server]] · [[proj-board-game-barrister-bopis]] · [[bigcommerce-catalyst]] · [[fly-io]]
