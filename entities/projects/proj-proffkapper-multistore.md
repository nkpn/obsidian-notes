---
type: entity
category: project
tags: [shopify, liquid, dawn, multistore, theme-architecture, webpack]
project-platform: [shopify-plus]
client: proffkapper
repo: Proffkapper.no
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# Proffkapper — Speroteck Multi-Store Theme Architecture

Speroteck-exclusive [[liquid]] theme architecture for **multi-site Shopify clients**. Inspired by the cartridge architecture from Salesforce Commerce Cloud (SFCC). All sites build on top of [[#dawn]]; site-specific overrides cascade in three tiers.

Author: Nikita Ilin. Internal name: `shopify_multistore`. **This is the "internal multi-store theme system that accelerates new-store launches" referenced in the CV** ([[2026-05-05-02-cv-merge-speroteck]]).

## Cartridge model

Three layers, top-down at build time:

1. **Site cartridge** (e.g., `proffkapper/`) — site-specific overrides.
2. **`_shared/` cartridge** — Speroteck core overrides shared across all client builds.
3. **Dawn** — Shopify's reference theme, fetched fresh via `dawn:setup`.

The bundler walks top-down — first match wins. Adding a new site means: drop a new folder under `sites/`, override only what diverges, build.

## Pipeline (evidence from `package.json`)

| Script | What it does |
|---|---|
| `dawn:setup` | `download-dawn.js` then `webpack` against `webpack.dawn.config.js` — fetches the latest Dawn baseline. |
| `theme:watch` | Parallel: `create-deploy-folder` (builds the layered cartridge into `./deploy/`) + `watch-deploy-folder`. |
| `theme:dev` | `shopify theme dev --environment client-develop` against the deploy folder. |
| `theme:bundle-dev` | Webpack-builds the shared JS bundle from `sites/_shared/js/bundle/` into `deploy/assets/index.bundle.js`. |
| `theme:download` | Pulls live theme code, splits files back into the right cartridge folders (`.liquid` → `liquid/`, `.js` → `js/`, `.css` → `styles/`). |
| `theme:check` | `shopify theme check` for liquid linting. |

## Stack

- Webpack 5 + Babel for the shared JS bundle.
- Tailwind 3 + PostCSS + Sass.
- `@shopify/prettier-plugin-liquid` for Liquid formatting.
- Husky + lint-staged + stylelint for pre-commit.

## Senior signal

- **"Why not just one theme per store?"** — onboarding cost. With 5+ sister stores, every shared bug fix would otherwise be N PRs. Cartridge model = 1 PR to `_shared`, all sites pick it up next build.
- **"Why Dawn as the bottom layer instead of Skeleton/Horizon?"** — Dawn was the production-stable reference at the time the architecture was designed; Horizon is newer (see [[proj-horizon-theme]]) and changes more aggressively. The pull-from-upstream design lets you swap the base without rewriting overrides.
- **"How does this interact with theme app extensions?"** — app blocks live alongside the cartridge; the build doesn't fight them since app blocks aren't files in the theme folder.

## War story shape

A site-level CSS override stopped applying after a Dawn upstream pull. Theory: cartridge build order. Evidence: webpack resolution showed `_shared/styles/component.scss` was being picked before `proffkapper/styles/component.scss` because the override file had been renamed. Fix: kept the original filename in the site cartridge, added a comment about why naming matters. One-line fix; would have been a half-day debug without the cartridge mental model.

## Related

[[liquid]] · [[shopify-plus]] · [[speroteck]] · [[proj-horizon-theme]]
