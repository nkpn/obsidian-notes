---
type: entity
category: project
tags: [shopify, liquid, theme]
project-platform: [shopify-plus]
client: aluline
repo: aluline-shopify
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# Aluline — Shopify Liquid Theme

[[liquid]] theme for Aluline. Standard OS 2.0 sections + JSON templates structure (no separate package.json — pure theme repo, deployed via Shopify CLI).

## Evidence

- Sections include: `carousel.liquid`, `featured-product.liquid`, `featured-product-information.liquid`, `featured-blog-posts.liquid`, `collection-list.liquid`, `collection-links.liquid`, `layered-slideshow.liquid`, `divider.liquid`, `custom-liquid.liquid`, `hero.liquid`, `logo.liquid`, plus product/blog/cart/page main templates.
- Templates: full standard set (`product.json`, `collection.json`, `cart.json`, `blog.json`, `article.json`, `page.json`, `password.json`, `404.json`, plus `page.coming-soon.json`, `page.contact.json`, `gift_card.liquid`, `list-collections.json`).
- Settings (`config/settings_data.json`): Montserrat font family across body/headings/accents, 48px H1 baseline, monochrome-leaning typography. Logo height 69px.

## Pair with

The companion repo [[proj-aluline-nextjs]] is a Next.js + Firebase marketing site for the same brand — likely the lead-gen / pre-launch surface, while this Shopify theme is the actual storefront. Senior interview play: "We split brand marketing pages from commerce pages so SEO/CMS workflows don't fight Shopify theme deploys."

## Related

[[liquid]] · [[shopify-plus]] · [[proj-aluline-nextjs]] · [[speroteck]]
