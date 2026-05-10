---
type: entity
category: project
tags: [nextjs, next-intl, agency-site, i18n]
client: nordly-digital
repo: nordly-digital
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# Nordly Digital — Next.js i18n Agency Site

Next.js 14 site for Nordly Digital with `next-intl` for full i18n routing (`app/[locale]/...`). Two locales: English and Norwegian (`messages/en.json`, `messages/no.json`).

## Stack (evidence)

- Next.js 14.2.5, App Router.
- `next-intl` 4.11 — locale-aware routing + ICU-style message strings.
- Tailwind 3 + TypeScript 5.
- `middleware.ts` — most likely for locale detection / redirect.

## Repo conventions

The repo carries an `AGENTS.md` with the same `/graphify` knowledge-graph rule the user enforces in this Obsidian vault. Senior signal: AI-agent-friendly repo conventions are checked in, not retrofitted.

## Interview value

- **"How do you do i18n at the routing layer in App Router?"** → next-intl's `[locale]` segment + middleware redirect for locale detection + per-locale JSON message files. Server Components opt into i18n via `getTranslations()`; Client Components use the React provider. Knowing the boundary is a senior tell.

## Related

[[speroteck]] · [[nikita-ilin]]
