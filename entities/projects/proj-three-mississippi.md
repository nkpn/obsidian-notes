---
type: entity
category: project
tags: [shopify, liquid, dawn, theme, food-and-beverage]
project-platform: [shopify-plus]
client: three-mississippi
repo: three-mississippi-shopify
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# Three Mississippi — Dawn-Based Shopify Theme

Custom Dawn-based [[liquid]] theme for the Three Mississippi store. Food/beverage/restaurant vertical (evidence: `templates/page.restaurant.json`, `templates/article.recipe.json`, `templates/blog.recipes.json`, `templates/product.bundle.json`).

## Shape

- Dawn fork, customized via OS 2.0 sections + JSON templates.
- Specialized templates: restaurant page, recipe articles + recipe blog, product bundles, accessories category.
- Notable sections (evidence from `sections/`): `age-check.liquid`, `comparison-table.liquid`, `collection-focus-carousel.liquid`, `feature-logo-with-image.liquid`, `faq.liquid`, `curved-text.liquid` — clearly a **content-rich brand site**, not a basic catalog.
- Has `readymag.html` — likely a marketing landing pulled in from Readymag and embedded.

## Senior signal — vertical-specific theme work

- **Recipes as articles**: `article.recipe.json` is a separate template variant. Shopify's metaobjects are usually the right substrate for structured content (ingredients, steps), but for a content team that lives in the article editor, custom article templates with section-driven blocks are the lower-friction path. Senior decision = match the editorial tool the brand actually uses.
- **Age-check section**: alcohol-adjacent content. Senior implementation does this in Liquid + cookies, not JS-only — bots can't dismiss a Liquid-rendered gate.
- **Bundle product template**: product.bundle.json with its own section tree handles bundle-specific UX (bundle composition, savings displayed). Mid-level mistake = trying to fork the standard product template; senior move = a separate template assigned per product.

## Related

[[liquid]] · [[shopify-plus]] · [[speroteck]] · [[proj-aluline-shopify]]
