---
type: entity
category: project
tags: [koa, proxy, fly-io, real-estate, static-egress-ip, integration]
repo: booking-proxy-fly-io
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# Booking Proxy — Resales Online V6 (Static Egress IP)

Koa proxy in front of **Resales Online V6** real-estate API, deployed on [[fly-io]] specifically to get a **static outbound IPv4** that satisfies the upstream API's IP allowlist.

Two repos: `booking-proxy` (older, generic) and `booking-proxy-fly-io` (the deployed one).

## Stack (evidence)

- Koa 2 + `koa-router` + `koa2-cors`, `node-fetch` 2, `config` for layered env.
- Endpoints: `GET /searchProperties`, `GET /propertyDetails`.
- Dockerfile + `fly.toml`.

## The infra trick (the actual interview value)

Resales Online's API key auth is **IP-allowlisted on the API key itself**. Without a stable outbound IP, every redeploy reshuffles the egress IP and the API key 401s. Fly.io's solution:

```bash
fly ips allocate-egress -a booking-proxy-fly-io -r ams
fly scale count 1 --region ams -a booking-proxy-fly-io
```

- `allocate-egress` reserves a static IPv4 for outbound.
- `scale count 1 --region ams` pins the single machine to one region so failover doesn't change the source IP.
- Verification: `fly ssh console -C "curl -4 https://api.ipify.org"` to confirm the actual outbound IP, then paste into the API key allowlist.

## Senior signal

- **"Why not just use serverless?"** → Serverless platforms recycle IPs. Allowlisted upstreams can't trust a serverless-hosted client. Knowing this trade-off cold is a senior tell.
- **"Why a proxy at all?"** → API key shouldn't ship to the browser, and CORS on Resales Online would block direct browser calls anyway. The proxy is both an auth boundary and a CORS-fix.
- **"What's the failure mode?"** → If Fly migrates the machine to a new host (rare but possible), egress IP can change. Monitoring = ping ipify periodically and alert on change; rotation = update the API key allowlist before swapping.

## Related

[[fly-io]] · [[nikita-ilin]] · [[speroteck]]
