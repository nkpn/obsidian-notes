---
type: moc
updated: 2026-05-10
---

# Vault Index

Map of Content for the session DB. Treat this as the entry point — Claude reads this first to figure out what's already known before starting a new session.

## How this works

- **`sessions/`** — one note per Claude session. Each has a TL;DR, decisions, and `[[entity links]]`. **Reload only the TL;DR + linked entities to save tokens** instead of replaying full transcripts.
- **`entities/`** — atomic notes for projects, people, concepts, files. The graph nodes.
  - **`entities/projects/`** — one note per working project / repo (interview-prep deep dives).
- **`react-lessons/`** — your existing React interview prep notes.
- **`Index.md`** — this file. The MOC / "global summary" Claude reads first.

## Latest sessions

- 2026-05-10 — [[2026-05-10-01-projects-portfolio-audit]] — audited all 19 working repos; built per-project entities + 6 new concept entities for senior Shopify interview prep (current).
- 2026-05-05 — [[2026-05-05-01-obsidian-session-db-setup]] — set up this whole graph-shaped session DB.
- 2026-05-05 — [[2026-05-05-02-cv-merge-speroteck]] *(backfill)* — merged + recruiter-tuned the Speroteck CV.
- 2026-05-05 — [[2026-05-05-03-shopify-interview-prep]] *(backfill)* — built study guide / Q&A / system design docs for senior Shopify interview.

## Sessions by tag

- `#career` → [[2026-05-05-02-cv-merge-speroteck]], [[2026-05-05-03-shopify-interview-prep]], [[2026-05-10-01-projects-portfolio-audit]]
- `#interview-prep` → [[2026-05-05-03-shopify-interview-prep]], [[2026-05-10-01-projects-portfolio-audit]]
- `#shopify` → [[2026-05-05-02-cv-merge-speroteck]], [[2026-05-05-03-shopify-interview-prep]], [[2026-05-10-01-projects-portfolio-audit]]
- `#bigcommerce` → [[2026-05-05-02-cv-merge-speroteck]], [[2026-05-10-01-projects-portfolio-audit]]
- `#portfolio` → [[2026-05-10-01-projects-portfolio-audit]]
- `#knowledge-management` → [[2026-05-05-01-obsidian-session-db-setup]]
- `#graphrag` → [[2026-05-05-01-obsidian-session-db-setup]]

## Key entities

**Concepts / Patterns**
- [[graphrag]] · [[obsidian-knowledge-graph]] · [[bopis]] · [[app-proxy-pattern]]

**Platforms**
- [[shopify-plus]] · [[bigcommerce-catalyst]] · [[oxygen]] · [[fly-io]]

**APIs / Frameworks / Languages**
- [[hydrogen]] · [[shopify-functions]] · [[liquid]] · [[checkout-ui-extensions]] · [[customer-account-ui-extensions]]

**Companies / People**
- [[speroteck]] · [[ipc2u]] · [[nikita-ilin]]

## Project entities (`entities/projects/`)

**Tier 1 — Top Shopify firepower**
- [[proj-cole-haan-customer-account-ui]] — Shopify Plus customer-account UI extensions, Global-E.
- [[proj-bloomreach-pos-connector]] — Shopify App: POS UI + checkout UI, Bloomreach CDP.
- [[proj-kindwater-headless-middleware]] — Hydrogen-as-middleware on Oxygen, HMAC.
- [[proj-checkout-extensions-gift-message]] — checkout + customer-account extensions via metafields.
- [[proj-ai-description-generator]] — Shopify App + OpenAI on RR v7.
- [[proj-proffkapper-multistore]] — Speroteck Dawn-cartridge multistore architecture (CV claim).
- [[proj-shopify-storefront-test]] — Next.js + Storefront API alternative to Hydrogen.

**Tier 2 — Shopify themes**
- [[proj-horizon-theme]] — Shopify's 2026 flagship theme baseline.
- [[proj-three-mississippi]] — Dawn-based food/recipe vertical theme.
- [[proj-aluline-shopify]] — OS 2.0 Liquid theme.

**Tier 3 — BigCommerce**
- [[proj-bigcommerce-bopis-server]] — Express+Fly.io BOPIS API service.
- [[proj-board-game-barrister-bopis]] — Stencil/Cornerstone with BOPIS frontend.

**Tier 4 — Backend / integration / training**
- [[proj-nestjs-orders-api]] — NestJS GraphQL + RabbitMQ retry/DLQ pipeline.
- [[proj-nest-ecomm-rd]] — Modular NestJS + gRPC payments microservice.
- [[proj-booking-proxy-resales]] — Koa+Fly static-egress-IP proxy for Resales Online.
- [[proj-mini-nestjs]] — Nest-from-scratch (framework internals).

**Tier 5 — Adjacent**
- [[proj-aluline-nextjs]] — Next.js + Firebase marketing site (paired with aluline-shopify).
- [[proj-nordly-digital]] — Next.js + next-intl agency site.
- [[proj-mymarbella]] — older webpack + jQuery real-estate site.

## Conventions (so future-Claude doesn't drift)

1. **Filenames** — `YYYY-MM-DD-NN-slug.md` for sessions; project entities prefixed `proj-` and live in `entities/projects/`.
2. **Frontmatter is required** on session and entity notes. Sessions need `date`, `tags`, `project`, `entities`, `tldr`, `type: session`. Entities need `type: entity`, `category`, `tags`, `mentioned-in`. Project entities also carry `repo` and (where known) `client`.
3. **Entity notes are atomic** — one concept per note. Heavy linking via `[[wikilinks]]`, no duplication.
4. **TL;DR rule** — every session note opens with a ≤5-sentence TL;DR. That's what Claude reloads in future sessions.
5. **No transcript dumps** — capture decisions, deltas, open threads. Not chat logs.
6. **Backfills** are tagged `#backfill` in frontmatter so they're distinguishable from native sessions.
7. **War-story shape** — interview-prep entities label inferred war stories explicitly so they aren't mistaken for verified facts. Replace with real recall before interview day.
