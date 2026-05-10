---
type: entity
category: platform
tags: [hosting, edge, infra]
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# Fly.io

Hosting platform for small Dockerized services close to the edge. Nikita uses it as the default home for "needs to be a real backend, not a serverless function" services.

## Why it gets reached for

- **Single-region pinning + static egress IPv4** — lets the service satisfy upstream API allowlists (e.g., Resales Online V6 in [[proj-booking-proxy-resales]] requires the source IP to match the API key).
- **Always-on machine** for low-latency third-party APIs where cold starts hurt UX.
- **`fly secrets set`** flow for env management — no separate vault.
- **`fly.toml` + Dockerfile** is the entire infra spec; deploys from CLI, no CI required.

## Used in

- [[proj-bigcommerce-bopis-server]] — BOPIS backend (`bigcommerce-server-bopis-fly-io.fly.dev`).
- [[proj-booking-proxy-resales]] — Resales Online proxy (Koa + fixed egress IP).

## Senior signal

Pick Fly when the constraint is "stable outbound IP + always-on small service"; pick [[oxygen]] / Cloudflare Workers when the constraint is "free and globally edge". Don't conflate them.

## Related

[[oxygen]] · [[proj-bigcommerce-bopis-server]] · [[proj-booking-proxy-resales]]
