---
type: entity
category: project
tags: [configurator, artifi, astro, react, state-bridge, mobile, rebuild]
client: unknown
repo: unknown
mentioned-in: [[2026-05-14-02-interview-self-introduction-prep]]
---

# Artifi Product Configurator

Heavily customizable PDP rebuild combining the storefront, a separate Astro + React design-editor app, and the Artifi SDK canvas tool. All three contexts synchronize through a `productStateManager` placed on the `window` object — single source of truth for price, option, and quantity changes; both the storefront and the design tool subscribe to it.

## Architecture

- **Three contexts, one bridge:** storefront ↔ Astro design editor ↔ Artifi canvas SDK, all synchronized via `productStateManager` (window object). Each side has subscribers reacting to price/option/qty changes.
- **Metadata-driven PDP:** customization steps (Color, Print Method, etc.), option configurations (image upload, text, color), and option dependencies (visibility based on prior selections) are all configuration, not code. Adding a new step is metadata, not a refactor.
- **Mobile subscriber wrapper:** Artifi SDK exposes very limited widget states for small screens, so a custom subscriber layer wraps the SDK to manage UI transitions on mobile rather than fork the SDK.
- **Standard flow:** user clicks "Design Now" on the storefront → browser navigates to the Astro app → loads React UI + Artifi SDK canvas.

## Why it existed (rebuild context)

Replacement for an earlier configurator that had grown organically. Symptoms of the old version (per the user's own framing): state-sync bugs between canvas and cart causing wrong-price / wrong-design checkouts, fragile mobile flow with sessions breaking mid-customization, mismatched orders requiring refunds or reprints. The rebuild thesis: smaller migration unit per change, no surprises in production, mobile parity.

## Senior beats (interview material)

- "Three systems, one source of truth" — the architectural one-liner.
- "Rather than fork the SDK, I wrapped it in my own subscriber layer" — when-not-to-fight-third-party-tools tradeoff.
- "New customization SKUs ship without code changes" — ties architecture to business impact, **conditional on actually being true** for the project.

## Open verification

- Was `window.productStateManager` typed end-to-end, or did the two sides drift on what shape `option` was? (This is a likely follow-up question in interviews.)
- What test strategy actually shipped — mocked subscribers in isolation, integration tests, manual QA only?
- Real impact metrics — abandonment delta, mobile completion delta, support-ticket volume on "my order doesn't match my design" — are not yet sourced. Don't claim percentages in interview without a defensible source.

## Related

[[shopify-plus]] · [[checkout-ui-extensions]]
