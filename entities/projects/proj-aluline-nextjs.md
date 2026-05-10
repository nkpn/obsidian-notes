---
type: entity
category: project
tags: [nextjs, firebase, marketing-site, react]
repo: aluline-nextjs
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# Aluline — Next.js Marketing Site (Firebase)

Next.js 14 marketing/lead-gen site for the same brand whose Shopify storefront is in [[proj-aluline-shopify]]. Pages-router architecture (`pages/`), Firebase backend (likely Firestore for form submissions + Functions), `nodemailer` for outbound email, SWR for client-side fetching.

## Stack (evidence)

- Next.js 14.1.4 + React 18.
- Firebase 10 (Firestore + Auth probably).
- `nodemailer` 6.9 — server-side transactional email from `pages/api/`.
- `swr` 2 — client-side cache.
- `pages/api/` directory + `lib/firebase.js`, `lib/i18n.js`, `lib/useTranslation.js` (custom i18n hook).

## Pair with

[[proj-aluline-shopify]] — the actual storefront. Splitting marketing from commerce keeps Shopify theme deploys decoupled from content team workflow.

## Senior signal — the "split brand vs. commerce" play

- **"Why two stacks instead of putting marketing pages on Shopify?"** → Shopify pages are tied to theme deploys; marketing teams want to ship copy hourly. Next.js + Firebase gives them a CMS-shaped editing surface without touching the Liquid theme.
- **"How do you keep the brands consistent?"** → Shared design tokens, shared header/footer markup mirrored, but the rendering stacks are separate. SEO-wise, careful canonicalization between the two domains.
- **When this is wrong**: small brands without dedicated marketing teams — the operational cost of two stacks isn't worth it.

## Related

[[proj-aluline-shopify]] · [[speroteck]] · [[nikita-ilin]]
