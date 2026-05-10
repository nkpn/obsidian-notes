---
type: entity
category: project
tags: [shopify, liquid, horizon, theme-blocks, dawn-successor]
project-platform: [shopify-plus]
repo: horizon
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# Horizon Theme

Shopify's flagship 2026 first-party theme — the successor reference architecture to [[#dawn]]. Built around **theme blocks** (`shopify://docs/storefronts/themes/architecture/blocks/theme-blocks`), aggressive use of evergreen-web features, server-rendered Liquid with progressive enhancement.

This repo is a fork/customization base — Shopify's stance is explicit: themes derived from Horizon are **not eligible** for Theme Store submission. So the use case here is custom client builds, not theme-store distribution.

## Why it matters for an interview

- Horizon represents the post-Dawn theme architecture: theme blocks replacing rigid sections, JSON templates plus block trees, async/on-demand rendering as a progressive enhancement.
- The principles in the README are interview gold: "web-native", "lean and reliable", "server-rendered (Liquid renders, JS enhances)", "functional, not pixel-perfect".
- 2026-only Liquid APIs may surface in `main` before docs catch up — knowing to read the source instead of waiting for docs is a senior tell.

## Senior signal

- **"When would you start a new client on Horizon vs Dawn vs custom?"**
  - Horizon: client wants modern theme blocks, willing to track upstream changes.
  - Dawn: stable, well-documented baseline; safer for clients who don't want to chase upstream.
  - Custom (e.g., the [[proj-proffkapper-multistore]] cartridge): multi-site organizations where shared overrides matter more than upstream pulls.
- **"Why server-rendered first?"** — translations, money formatting, B2B catalog logic — all server concerns. Putting them in JS gives you bugs at every locale and tax-zone boundary. Horizon's defaults reinforce this discipline.

## Related

[[liquid]] · [[shopify-plus]] · [[proj-three-mississippi]] · [[proj-proffkapper-multistore]]
