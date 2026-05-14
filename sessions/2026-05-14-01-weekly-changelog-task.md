---
date: 2026-05-14
session_id: local_85d7ee30-6cd1-4dbe-85bd-ba0c6b0e8f14
type: session
project: workstation-hygiene
tags: [scheduled-tasks, shopify, bigcommerce, changelog, automation]
entities: [[nikita-ilin]], [[shopify-plus]], [[bigcommerce-catalyst]]
tldr: Set up a recurring Monday 9am task that pulls developer changelogs from shopify.dev/changelog and developer.bigcommerce.com/changelog, filters to the past 7 days, groups by platform and API area, and flags breaking changes / deprecations / new API versions at the top. First scheduled run is Monday 18 May 2026. The task is constrained to the official portals only (no third-party blogs).
status: complete
auto-saved: true
---

# Weekly Shopify + BigCommerce Changelog Task

## TL;DR

Recurring scheduled task created so Nikita gets a Monday-morning summary of new entries from the two official developer changelogs without having to remember to check them. Source pages are locked to shopify.dev/changelog and developer.bigcommerce.com/changelog — no aggregators, no third-party rewrites. Output is grouped by platform and API area, with breaking changes / deprecations / new API versions surfaced at the top. First scheduled run is Monday 18 May 2026 around 9:02 AM.

## Decisions

- Official portals only. The user explicitly rejected third-party aggregators after the initial draft, so the WebFetch scope is pinned to the two source pages.
- Weekly cadence on Monday morning, not daily — keeps signal-to-noise sane and aligns with the start of the work week.
- Sort by impact, not chronology. Breaking changes / deprecations / new API versions surface first; informational updates last.
- Run-now-once tip surfaced for permission pre-approval: WebFetch on the two domains needs a one-time approval, otherwise the first scheduled run pauses waiting.

## Deliverables

- Scheduled task active, next run Monday 18 May 2026 ~9:02 AM local.
- Sources fixed to: https://shopify.dev/changelog and https://developer.bigcommerce.com/changelog

## Open threads

- WebFetch permission for the two domains needs to be pre-approved by hitting Run now once — flagged but not yet done.
- No archival of past summaries — each run is one-shot. If a longer-term changelog journal is wanted, that's a follow-up to add (e.g. append each run to a notes/changelog-feed.md).

## Linked entities

[[nikita-ilin]] · [[shopify-plus]] · [[bigcommerce-catalyst]]
