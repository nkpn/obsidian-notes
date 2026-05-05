---
type: entity
category: language
tags: [shopify, templating, themes]
mentioned-in: [[2026-05-05-03-shopify-interview-prep]]
---

# Liquid

Shopify's server-side templating language. The substrate of Online Store themes.

## What's still in Liquid in 2026

- Theme rendering (Online Store 2.0 sections, blocks, JSON templates).
- Email notifications.
- Theme app extensions (app blocks).

## What's leaving Liquid

- **checkout.liquid** — being phased out for non-Plus and migrated to checkout UI extensions on Plus.
- Storefront-side data fetching for any non-trivial UX → moving to [[hydrogen]] or app proxies.

## Senior signal

Liquid's biggest gotcha: it runs **server-side per request** with no client interactivity. Stuffing logic into Liquid that should live in JS or in an app is the classic mid-level mistake.

## Related

[[shopify-plus]] · [[hydrogen]] · [[shopify-functions]]
