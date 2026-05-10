---
type: entity
category: project
tags: [webpack, jquery, handlebars, real-estate, legacy]
repo: mymarbella
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# MyMarbella / Zirka Real Estate

Older static-build real-estate site (zirka.agency / Marbella, Spain). Webpack + Babel + Sass + Handlebars templates + jQuery + Bootstrap 4. Likely talks to [[proj-booking-proxy-resales]] indirectly for property data.

## Stack (evidence)

- Webpack 5 + Babel 7 + Sass + PostCSS.
- jQuery 3, Bootstrap 4, Slick carousel, Bootstrap-Select.
- Handlebars (via `handlebars-loader`).
- `gh-pages` deploy target — static site to GitHub Pages.
- Axios + `spin.js` for AJAX UI.

## Interview framing

This isn't a Shopify project — it's a "career-arc" data point. If asked "what's your oldest still-running project?" or "show me how your stack has evolved", this is the contrast: jQuery + Webpack + Handlebars (2022-era patterns) vs. the React Router v7 + Hydrogen + Oxygen of [[proj-bloomreach-pos-connector]] / [[proj-kindwater-headless-middleware]] (2026 patterns).

## Senior signal

Owning that you've shipped older stacks isn't a weakness — it's evidence you know **why** the modern alternatives exist (e.g., why Server Components beat jQuery DOM mutation, why CSS-in-JS or Tailwind beat Bootstrap class soup, why structured CMS beats Handlebars partials). Don't pretend you only ever shipped 2026 stacks.

## Related

[[proj-booking-proxy-resales]] · [[nikita-ilin]]
