---
type: entity
category: project
tags: [bigcommerce, stencil, cornerstone, bopis, frontend]
project-platform: [bigcommerce-catalyst]
client: board-game-barrister
repo: board-game-barrister-old-version
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# Board Game Barrister — Stencil BOPIS Frontend

BigCommerce **Stencil** (Cornerstone v4.5) theme for Board Game Barrister, with a fully implemented [[bopis]] cart + checkout flow. Pairs with [[proj-bigcommerce-bopis-server]] on Fly.io.

The repo carries its own architecture doc: `BOPIS-Technical-Documentation.md`.

## Stack

- `@bigcommerce/stencil-cli` 8.10, `@bigcommerce/stencil-utils` 6.20.
- jQuery + Foundation 5 (Cornerstone heritage), Webpack 4, Babel.
- `@bigcommerce/citadel` UI library, `slick-carousel`, `easyzoom`, `sweetalert2`, `nod-validate`.
- Lighthouse + Jest, ESLint airbnb config.
- Grunt for build orchestration alongside webpack.

## BOPIS frontend (evidence from `BOPIS-Technical-Documentation.md`)

### Cart workflow (`initializeBOPIS()`)

1. Read cart line items → product IDs.
2. `fetchInventoryDetails(productIDs)` → calls `https://bigcommerce-server-bopis-fly-io.fly.dev/api/inventory/items` ([[proj-bigcommerce-bopis-server]]).
3. `prepareAvailableForSellAndPickupLocations()` — cross-references inventory against pickup-eligible locations.
4. If no pickup-eligible locations → hide shipping method selector entirely.
5. Otherwise, `getCurrentLocation()` (geolocation API → IP fallback) ranks the store list.

### Checkout workflow

- Posts to the proxy's `/api/checkout/:id/consignments` to swap shipping for pickup.
- Updates use `PUT` against the consignment ID — not delete-and-create — to keep the cart token alive.

## Senior signal

- **Why the workflow stops on no pickup locations**: BOPIS can't gracefully degrade to "no method selected"; the only valid states are "ship" or "pickup at known store". Mid-level implementations leave both options visible and break at consignment-add.
- **Geolocation fallback to IP**: browser geolocation prompts get denied frequently. Senior dev adds the IP fallback pre-emptively; mid-level finds out from a support ticket.
- **Stencil constraint articulation**: Stencil's `pages/cart` PageManager is the right injection point — the `cart_loaded` event from Stencil-utils fires after BigCommerce repaints the cart, and BOPIS state has to re-init then.

## War story shape

After BigCommerce released a cart-update API change, the BOPIS state stopped re-initializing after AJAX cart updates. Theory: cart event hook regression. Evidence: Stencil-utils 6.x renamed the event payload signature; `cart_loaded` fired but `data.line_items` was now nested. Fix: surgical — update the destructure, add a contract test against the new payload shape, ship.

## Related

[[bopis]] · [[bigcommerce-catalyst]] · [[proj-bigcommerce-bopis-server]] · [[speroteck]]
