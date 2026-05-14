---
type: entity
category: concept
tags: [performance, seo, web-vitals, lcp, inp, cls]
mentioned-in: [[2026-05-14-02-interview-self-introduction-prep]]
---

# Core Web Vitals

Google's user-experience metrics used for both ranking signals and "is this page actually fast?" judgement. Three primary metrics today (LCP, INP, CLS) plus supporting metrics (FCP, TTFB, TBT). Targets are at the **75th percentile of real users** — not the average, not a single Lighthouse run.

## Primary metrics

- **LCP — Largest Contentful Paint:** when the largest above-the-fold element finishes rendering. **Target < 2.5s.** On a Shopify PDP that's almost always the hero image. Killed by lazy-loading the LCP image, missing preload links, render-blocking CSS/JS in the head.
- **INP — Interaction to Next Paint:** how fast the page responds across **all** interactions in a session, not just the first. Replaced FID in March 2024. **Target < 200ms.** Killed by third-party app scripts running long tasks on the main thread, heavy variant selectors that re-render the whole PDP.
- **CLS — Cumulative Layout Shift:** total unexpected layout movement. **Target < 0.1.** Killed by images without explicit dimensions, late-loading web fonts, app embed blocks injecting banners above existing content.

## Supporting metrics

- **FCP — First Contentful Paint:** any first paint. < 1.8s.
- **TTFB — Time to First Byte:** server response. < 800ms. On Online Store this is largely Shopify's edge; on Hydrogen/Oxygen, your loaders matter.
- **TBT — Total Blocking Time:** lab-only proxy for INP. Sum of long tasks (>50ms). < 200ms.

## Lab vs field — the senior distinction

- **Lab** (Lighthouse, WebPageTest): single synthetic run. Useful for diagnosis only.
- **Field** (CrUX, `web-vitals` library, PageSpeed Insights field section): real users, 28-day rolling, 75th percentile. **This is what Google scores you on.**

Junior devs optimize the Lighthouse score. Senior devs optimize field data and know the two often disagree.

## Shopify-specific notes

- LCP usually fixed by preloading the hero image and serving as WebP/AVIF with the right `width=` param via Shopify CDN.
- INP usually fixed by auditing third-party app scripts (Klaviyo, reviews, chat) — that's where main-thread time disappears.
- CLS usually fixed by setting image dimensions and reserving space for app-injected content.
- Shopify's theme inspector for Chrome shows Liquid render time per section — Shopify-specific tool worth naming in interviews.

## Related

[[hydrogen]] · [[liquid]] · [[shopify-plus]]
