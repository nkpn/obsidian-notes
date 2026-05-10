---
type: entity
category: project
tags: [nestjs, learning, ioc, di, framework-internals]
repo: mini-nestjs
mentioned-in: [[2026-05-10-01-projects-portfolio-audit]]
---

# Mini-NestJS — Framework Internals from Scratch

Pet project: a minimal Nest-like framework built from scratch to learn **IoC/DI, HTTP layer, params, pipes, guards, interceptors, filters, modules**. Run with `npx ts-node src/app.ts`, listens on :3000.

## Why it's interview-worthy

Most NestJS devs use the framework without ever building a decorator-driven IoC container themselves. Demonstrating you can rebuild the abstractions = you understand them, not just `nest generate`.

The README ships with `curl` test cases that exercise:

- Basic route + per-method DI.
- Param + query pipes (`@Param`, `@Query` parsing + validation).
- BadRequest from a param pipe (number validation rejecting `abc`).
- Global `TrimPipe` on query strings.
- ZodValidationPipe on request body.

## Senior signal

- **"How does NestJS DI actually work?"** → Reflect-metadata + decorators register provider tokens; the module compiler walks the dependency graph at boot and resolves singletons (or scoped instances) into a container. Mid-level dev shrugs; senior dev can sketch it in 5 minutes.
- **"What's the difference between a pipe and a guard and an interceptor?"** → Pipes transform/validate values entering the handler; guards return boolean to allow/deny entry; interceptors wrap the request lifecycle (before + after). All three are easy to confuse until you've built them.

## Related

[[proj-nestjs-orders-api]] · [[proj-nest-ecomm-rd]] · [[nikita-ilin]]
