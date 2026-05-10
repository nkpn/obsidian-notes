---
type: entity
category: project
tags: [nestjs, graphql, rabbitmq, postgres, idempotency, prometheus, pet-project]
repo: NestJS-Docker-project
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# NestJS Orders API (Production-Ready Pet Project)

Production-shaped NestJS backend modelling an e-commerce **order pipeline**: GraphQL `createOrder` mutation → PostgreSQL persists `PENDING` → RabbitMQ → consumer processes → `COMPLETED`, with retry/DLQ. Built as a portfolio piece that demonstrates senior-level patterns interviewers actually probe.

## Stack (evidence from `package.json`)

- **NestJS 11** + Apollo GraphQL (`@nestjs/graphql` 13, `@apollo/server` 5).
- **PostgreSQL** + TypeORM 0.3.28; migrations with `typeorm-ts-node-commonjs migration:run`.
- **RabbitMQ** via `amqp-connection-manager` + `amqplib`; `@nestjs/microservices` for transport adapters.
- **JWT auth** (`@nestjs/jwt`, `passport-jwt`, `passport-local`) + `bcryptjs` + RolesGuard.
- **Bull + @nestjs/bull** for in-process queues alongside the AMQP transport.
- **Observability**: `@willsoto/nestjs-prometheus` + `prom-client` + `nestjs-pino` (pino structured logs) + `@nestjs/terminus` healthchecks.
- **Tests**: Jest 30, `@testcontainers/postgresql` for real-Postgres integration tests, contract/unit/e2e split.

## Architecture

```
Client (GraphQL)
  └─ NestJS App (GraphQL API, JWT + RolesGuard)
       └─ OrdersService
            ├─ transaction + SELECT ... FOR UPDATE   (oversell protection)
            ├─ idempotencyKey check                  (request-level dedup)
            └─ Postgres
              └─ RabbitMQ
                   ├─ order_queue
                   ├─ order_queue_retry  (TTL + DLX → order_queue)
                   └─ order_dlq
                       └─ OrdersConsumer
                            ├─ processed_messages insert  (exactly-once guard)
                            └─ COMPLETED / retry / FAILED + DLQ
```

## Senior-grade patterns to talk about

- **Two-layer idempotency**: API-level (`idempotencyKey` on the mutation) + consumer-level (`processed_messages` table with unique constraint). Different failure domains, different keys.
- **Oversell protection**: `SELECT ... FOR UPDATE` inside a transaction when decrementing inventory. Without it, two concurrent orders can both pass the "stock >= 1" check.
- **Retry topology**: `order_queue_retry` uses TTL + Dead Letter Exchange to bounce messages back into `order_queue` after a delay. After N retries, route to `order_dlq` for human triage. The DLQ is not a graveyard — it's a queue you alert on.
- **Schema hygiene**: TypeORM migrations checked in, `migration:revert` is wired up, every migration is `--transaction each` so a partial failure can't leave the schema drifted.
- **CI**: GitHub Actions badge in README — `ci.yml` runs the test pyramid.

## Senior signal (interview cues)

- "Why two queues plus a DLQ?" → because retries shouldn't block fresh traffic; segregating retries lets you tune TTL and DLX policies without touching the hot path.
- "Why idempotency on the consumer if the queue is at-least-once?" → because consumer redelivery + downstream side effects (charging payment, sending email) compound — without `processed_messages` the customer gets charged twice.
- "Why pino over Winston?" → JSON-first, async by default, faster, plays well with Loki/Datadog ingestion.

## Related

[[shopify-functions]] (constraint contrast: Functions are stateless and synchronous; this pipeline is the opposite design when you actually need state).

[[speroteck]] · [[nikita-ilin]]
