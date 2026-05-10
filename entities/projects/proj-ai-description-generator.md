---
type: entity
category: project
tags: [shopify, shopify-app, openai, react-router, polaris, prisma]
project-platform: [shopify-plus]
repo: shopify-app-test/ai-description-generator
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# AI Description Generator (Shopify App)

Embedded Shopify admin app that uses **OpenAI** to auto-generate product descriptions, then pushes them back to the catalog via Admin GraphQL. Clean small-app reference for "modern Shopify app" architecture.

## Stack (evidence-based)

- `@shopify/shopify-app-react-router` v1.1 (current template, post-Remix).
- React Router v7 (`@react-router/dev` 7.12) for SSR + routing.
- Polaris v13 + App Bridge React v4 — embedded admin UI.
- Prisma v6 + `@shopify/shopify-app-session-storage-prisma` v9 (durable sessions).
- `openai` v6 (official SDK).
- Node 20.19+ / 22.12+, ESM, Vite, Docker.

## Surfaces

- `app/routes/app.tsx` — Polaris shell.
- `app/routes/app._index.tsx` — landing inside the admin.
- `app/routes/app.generator.tsx` — generator UI (the actual feature).
- `app/services/ai.server.ts` — OpenAI client wrapper.
- `app/services/shopify-graphql.server.ts` — typed Admin API helpers.
- Webhooks: `webhooks.app.scopes_update.tsx`, `webhooks.app.uninstalled.tsx`.

## Senior signal

- **`.server.ts` boundary**: anything with credentials sits behind a `.server.ts` filename so RR's tree-shaker keeps it off the client bundle. OpenAI key never reaches the browser.
- **Why GraphQL for the writeback**: `productUpdate` mutation lets you batch description + SEO fields in one round-trip; REST would be 2+ calls.
- **Cost guardrails worth mentioning**: rate-limit per shop, cache by product `updatedAt` to avoid re-spending tokens on unchanged products, prompt versioning so an upgrade doesn't silently regenerate everyone's descriptions.

## When NOT to use it

For a single-merchant store, a Cloudflare Worker + cron + a single GraphQL mutation does the same job at zero infra. The full Polaris/RR app is justified when merchants want a UI to **review** AI output before applying it.

## Related

[[shopify-plus]] · [[speroteck]]
