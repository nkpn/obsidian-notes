---
date: 2026-05-10
session_id: local_be2ea55d-e806-473b-ab44-57b1a79ddb80
type: session
project: interview-prep
tags: [career, interview-prep, shopify, bigcommerce, portfolio]
entities: [[nikita-ilin]], [[speroteck]], [[shopify-plus]], [[hydrogen]], [[oxygen]], [[liquid]], [[shopify-functions]], [[checkout-ui-extensions]], [[customer-account-ui-extensions]], [[bopis]], [[app-proxy-pattern]], [[fly-io]]
tldr: Audited every working repo in `Documents/GitHub` (19 projects) for senior-Shopify-interview prep. Built one entity note per project plus six new concept entities (oxygen, customer-account-ui-extensions, checkout-ui-extensions, app-proxy-pattern, bopis, fly-io). Tier-1 interview firepower = Cole Haan customer-account extensions, Bloomreach POS connector, Kindwater Hydrogen-as-middleware, AI description generator, gift-message extensions, Proffkapper multistore cartridge. BigCommerce BOPIS (Fly.io server + Stencil frontend) is the strongest war story across both platforms.
status: complete
---

# Projects Portfolio Audit — Senior Shopify Interview Prep

## TL;DR

Walked all 19 working repos in `Documents/GitHub` and built evidence-based entity notes (`entities/projects/`) for each. Output is tuned for a senior Shopify-focused interview: Shopify projects get architecture + war-story shape, BigCommerce becomes supporting context, Node/NestJS/integration projects are reframed as breadth signals. Six new concept entities ([[oxygen]], [[checkout-ui-extensions]], [[customer-account-ui-extensions]], [[app-proxy-pattern]], [[bopis]], [[fly-io]]) tie projects together so future-Claude doesn't re-derive the patterns.

## Source

Real repos under `/Users/nkpn/Documents/GitHub`. Evidence base = `package.json`, READMEs, `shopify.extension.toml` files, source files for representative routes/extensions. No speculation; where the project shape required inference (e.g., war stories), the inference is marked as "war story shape" and should be replaced with Nikita's actual recall before interview day.

## Tiering (interview-value lens, Shopify-first)

### Tier 1 — Top Shopify firepower
- [[proj-cole-haan-customer-account-ui]] — enterprise Shopify Plus client, customer-account UI extensions, Global-E i18n integration, Preact, 2025-10 API.
- [[proj-bloomreach-pos-connector]] — full Shopify App on React Router v7 + Polaris + Prisma, POS UI tile + checkout UI coupon validator, Bloomreach CDP integration.
- [[proj-kindwater-headless-middleware]] — creative pattern: Hydrogen on [[oxygen]] as middleware proxy, HMAC-SHA-256 verification, three-context discriminated union, free hosting for app backend.
- [[proj-checkout-extensions-gift-message]] — checkout UI + customer-account UI round-trip via app-scoped metafields.
- [[proj-ai-description-generator]] — Shopify App + OpenAI integration, modern RR v7 template.
- [[proj-proffkapper-multistore]] — **the "internal multi-store theme system" from the CV** — SFCC-cartridge-inspired Dawn-based architecture for multi-site Shopify clients.
- [[proj-shopify-storefront-test]] — Next.js 16 + Storefront API as the "do you really need Hydrogen?" trade-off baseline.

### Tier 2 — Shopify themes
- [[proj-horizon-theme]] — Shopify's 2026 flagship theme (theme blocks, post-Dawn architecture).
- [[proj-three-mississippi]] — Dawn-based food/recipe vertical theme (custom restaurant + recipe + bundle templates).
- [[proj-aluline-shopify]] — standard OS 2.0 Liquid theme.

### Tier 3 — BigCommerce supporting context
- [[proj-bigcommerce-bopis-server]] — Express+Fly.io BOPIS API service (CV claim evidence).
- [[proj-board-game-barrister-bopis]] — Stencil/Cornerstone theme with full BOPIS frontend, calls the Fly.io server.

### Tier 4 — Backend / integration / training
- [[proj-nestjs-orders-api]] — production-shaped NestJS pet project (GraphQL, RabbitMQ retry/DLQ, idempotency, Prometheus, JWT).
- [[proj-nest-ecomm-rd]] — modular NestJS + gRPC payments microservice + S3 + WebSockets.
- [[proj-booking-proxy-resales]] — Koa+Fly.io static-egress-IP proxy for Resales Online V6 API (real-estate).
- [[proj-mini-nestjs]] — minimal Nest-from-scratch (IoC/DI/pipes/guards) — framework-internals signal.

### Tier 5 — Adjacent / older
- [[proj-aluline-nextjs]] — Next.js 14 + Firebase marketing site, paired with the Aluline Shopify theme.
- [[proj-nordly-digital]] — Next.js 14 + next-intl agency site with two locales.
- [[proj-mymarbella]] — older webpack + jQuery + Handlebars real-estate site (career-arc signal).

## New concept entities

- [[oxygen]] — Shopify's Workers-compatible edge runtime; non-obvious use as a middleware host.
- [[checkout-ui-extensions]] — checkout-side UI extension surface and 2026-04 upgrade context.
- [[customer-account-ui-extensions]] — post-purchase / account-side extension surface (Preact runtime).
- [[app-proxy-pattern]] — same-origin app backend pattern with HMAC-SHA-256 verification.
- [[bopis]] — buy-online-pickup-in-store concept node tying BC server + Stencil frontend.
- [[fly-io]] — when to choose Fly over Oxygen (static egress IP, always-on, etc.).

## Interview lead lines (pre-baked)

For "tell me about your work" → start with the dual-platform headline from [[2026-05-05-02-cv-merge-speroteck]] (30+ storefronts in 5 yrs, 20+ BigCommerce, 10+ Shopify Plus, no team), then pivot into:

1. **Architectural sophistication** → [[proj-kindwater-headless-middleware]] (Hydrogen-as-middleware on Oxygen for free).
2. **Enterprise Shopify Plus** → [[proj-cole-haan-customer-account-ui]] (Cole Haan + Global-E i18n branching).
3. **Real-time omnichannel** → [[bopis]] flow ([[proj-bigcommerce-bopis-server]] + [[proj-board-game-barrister-bopis]]).
4. **Multi-store ops scaling** → [[proj-proffkapper-multistore]] (cartridge architecture).
5. **Modern Shopify App stack** → [[proj-bloomreach-pos-connector]] (RR v7 + Prisma + dual extension types).

## Open threads (verify with Nikita before interview day)

- War-story specifics in each entity are "shape" — labelled as such. Replace with real recall before interview.
- Client list outside Cole Haan / Three Mississippi / Aluline / Board Game Barrister / Proffkapper / Kindwater / Bloomreach client / Nordly is unconfirmed — would help to map repo → client name where the repo name doesn't reveal it.
- The CV claim "10+ Shopify Plus stores from scratch" — we have direct evidence of ~5 in this repo set; the other 5+ may be in private/agency-only repos.
- A/B testing on AB Tasty (mentioned in CV) — no AB Tasty traces in any of the scanned repos. Probably theme-injected snippets that didn't make the repo. Worth surfacing as a separate war story.
- BOPIS Fly.io URL is currently public (`bigcommerce-server-bopis-fly-io.fly.dev`) — confirm whether it's still serving prod traffic, since interview answers will reference it.

## Output

- 19 project entities under `entities/projects/proj-*.md`.
- 6 new concept entities under `entities/`.
- This session note.
- Updated `Index.md` MOC to include all new sessions/entities.
- MD export at `/Users/nkpn/Documents/projects-portfolio.md` for offline reading.

## Linked entities

[[nikita-ilin]] · [[speroteck]] · [[shopify-plus]] · [[hydrogen]] · [[oxygen]] · [[liquid]] · [[shopify-functions]] · [[checkout-ui-extensions]] · [[customer-account-ui-extensions]] · [[bopis]] · [[app-proxy-pattern]] · [[fly-io]] · [[bigcommerce-catalyst]]
