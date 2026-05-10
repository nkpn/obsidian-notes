---
type: entity
category: project
tags: [shopify, storefront-api, nextjs, headless]
project-platform: [shopify-plus]
repo: shopify-plus-hydrogen-oxygen
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# Headless Storefront — Next.js + Storefront API (Connection Test)

Next.js 16 + React 19 scaffold that connects to the Shopify Storefront API directly using `@shopify/storefront-api-client`. Currently a connection test / minimal proof — first 10 products, success/failure status. Useful as a reference for "do I really need Hydrogen, or is a vanilla Next.js + Storefront API enough?"

Internal name: `headless-storefront`.

## Stack (evidence-based)

- Next.js 16.2.4, React 19.2 (RSC-first by default).
- `@shopify/storefront-api-client` 1.0.10.
- Tailwind 4 + ESLint 9 + TypeScript 5.
- API version `2026-04` (env-overridable: `NEXT_PUBLIC_SHOPIFY_API_VERSION`).

## Code shape

`lib/shopify.ts` builds the client and **throws fast** on missing env (`NEXT_PUBLIC_SHOPIFY_STORE_DOMAIN`, `NEXT_PUBLIC_SHOPIFY_STOREFRONT_TOKEN`) — matches Nikita's "throw errors / fail fast" preference.

`app/page.tsx` is a Server Component that runs the GraphQL query at request time, surfaces `errors.graphQLErrors` cleanly, and renders connected/failed states.

```typescript
const PRODUCTS_QUERY = `#graphql
  query ProductsForConnectionTest {
    products(first: 10) {
      nodes { id title handle }
    }
  }
`;
```

## Senior signal — "Hydrogen vs Next.js + Storefront API"

This repo is the live form of an interview-favorite trade-off discussion:

| | Hydrogen on Oxygen | Next.js + Storefront API |
|---|---|---|
| Hosting | Free, Shopify-managed | Vercel / your own |
| Edge runtime | Yes (Workers) | Yes (Vercel Edge / RSC) |
| Shopify-shaped helpers | `useOptimisticCart`, analytics, etc. | Build-your-own |
| Customer Account API | First-class | Manual wiring |
| Learning curve | Hydrogen + Remix idioms | Pure Next.js |
| Lock-in | Higher | Lower |

When NOT to reach for Hydrogen: small catalog + team already strong on Next.js + Vercel + you don't need Oxygen's free hosting. This repo is what that path looks like at minimum surface.

## Related

[[hydrogen]] · [[oxygen]] · [[shopify-plus]]
