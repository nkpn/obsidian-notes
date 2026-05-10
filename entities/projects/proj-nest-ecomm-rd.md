---
type: entity
category: project
tags: [nestjs, grpc, microservices, rabbitmq, websockets, s3, graphql, ddd]
repo: Nest-js-ecomm-RD
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# Nest-js-ecomm-RD — Modular E-Commerce R&D

R&D NestJS project building a modular e-commerce backend with **DDD-style module boundaries** plus a **gRPC payments microservice**. Sister project to [[proj-nestjs-orders-api]] but explores the broader architecture (multi-service, S3 uploads, realtime, GraphQL).

Internal name: `e-tech`. NODE-JS-PRO course context — built alongside coursework, surfaced in CV.

## Modules (evidence from `src/`)

- `auth` — JWT + Passport.
- `users`, `products`, `orders`, `orders-worker` — domain modules.
- `payments-service` — separate `nest start --entryFile payments-service/main` process.
- `rabbitmq` — AMQP transport.
- `realtime` — `socket.io` + `@nestjs/platform-socket.io` + `@nestjs/websockets` for live order updates.
- `files` — `@aws-sdk/client-s3` + `@aws-sdk/s3-request-presigner` for file uploads.
- `graphql` — Apollo + GraphQL Federation-friendly via `@nestjs/apollo`.
- `idempotency`, `health`, `seeds`, `migrations`, `common` — cross-cutting.

## gRPC payments microservice

`@grpc/grpc-js` + `@grpc/proto-loader`. Run with `npm run start:payments:dev`. Service split = orders own the order lifecycle, payments service owns payment intent creation + webhook callback handling — they communicate over gRPC for synchronous calls, RabbitMQ for fire-and-forget events.

## Performance scripts (evidence from `package.json`)

- `perf:seed` — generate orders dataset.
- `perf:load` — run hot scenario.
- `perf:baseline` — baseline measurement.

Having explicit perf scripts checked in is a senior tell — you're not waiting until prod to find the bottleneck.

## Senior signal

- **"Why a gRPC service for payments and not REST or events?"** → Payment intent creation is a strongly-typed synchronous request/response — you want a contract, schema validation, low overhead. gRPC fits; REST adds JSON serialization cost and weaker typing; events are wrong shape because the caller needs the result.
- **"Why a separate orders-worker module?"** → Orders module owns the API; orders-worker owns the consumer side. Same domain, different runtime concerns. You can scale them independently and the API process doesn't compete with the consumer for RabbitMQ channels.

## Related

[[proj-nestjs-orders-api]] · [[shopify-functions]] · [[nikita-ilin]] · [[speroteck]]
