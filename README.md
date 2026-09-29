# Kauan Pardini Augusto

Software engineer, backend and full stack. I build and operate production systems, and I measure what changes.
Curitiba, Brazil · final-year Software Engineering student at PUCPR · Java teaching assistant.

## Currently

**Beauty Max Tecnologia**, Software Developer (Apr 2026 – present). Multi-tenant SaaS for beauty businesses (Next.js, PostgreSQL, Redis, RabbitMQ) and the group's internal support platform (GLPI, NestJS, MariaDB).

- Designed the blue/green CI/CD the team ships with: 211 production releases in 11 weeks, a median of 20 minutes from merge to production, previous slot kept for rollback.
- Built invoice reconciliation against the payment provider: matches charges by checkout metadata, runs dry by default, is safe to re-run, and sends anything it cannot correlate to human review instead of guessing by amount.
- Made per-operator cash registers safe under concurrent use (row lock on opening, unique index returning 409 instead of 500).
- Built WhatsApp inbound processing (webhook → RabbitMQ → worker) with message-ID dedupe in Redis plus a unique index, without dropping retries.
- Migrated the help desk to containerized GLPI 11 on MariaDB; a full rehearsal with identical row counts measured a ~4-minute cutover window against a 30–45 minute estimate.
- Built a WhatsApp-to-ticket service (NestJS, Kysely, transactional outbox, BullMQ) and PHP plugins that open expiring remote-access sessions from inside a ticket.
- Fixed silent edit loss in a self-hosted Docmost fork (Yjs collaboration): dropped frames now resync in about a second.

## ConstruXion, my own SaaS project

A construction-management SaaS in production at [construxion.com.br](https://www.construxion.com.br). It is a modular monolith by design (NestJS + Next.js, 35 API modules) with shared-schema tenant isolation, run as 32 Docker Compose services across production, staging and observability on a self-hosted VM behind Cloudflare. I migrated it off Fly.io and kept Fly as a standby behind a Cloudflare Worker failover. The code is private, so here are the results, each measured before and after the change.

| Change | Before | After | How |
| --- | ---: | ---: | --- |
| Dashboard p99 at 12 req/s | 3.3 s | 210 ms | Released the SSR payload right after render; the web tier had been restarting on heap under load |
| HTTP response for a 1,000-row import | 7.0 s | ~3 ms | Commit moved to a BullMQ worker with idempotent job IDs, status polling and a dead-letter queue |
| First request after an idle period | 110 ms | 20 ms | Kept the Postgres pool warm; the driver's 10 s idle timeout was draining it |
| Cached page at the edge | ~60 ms | 28–41 ms | Edge cache in a Cloudflare Worker; normalized the Next.js `Vary` header that silently disabled caching |
| Managed database compute | 1.99 CU-h/day | ~0.07 CU-h/day | Primary moved to self-hosted Postgres with 5 days left on the quota; the managed copy stays for recovery |
| Restore from backup | – | 14–17 s | Hourly dumps to object storage, re-verified every month by an automated restore job |

Decisions I can walk through in an interview:

- The edge failover never replays a write the origin may already have processed, and it refuses to promote a stale standby.
- Every performance or cost change ships with a before-and-after record, including the ones that did not pay off.

## Projects

- [API-Rest-Java](https://github.com/J4kedi/API-Rest-Java): REST API in Java 21 and Spring Boot 3 with Spring Security/JWT, JPA, Flyway, OpenAPI and tests. Course-based study project.
- [ConstructionCon](https://github.com/J4kedi/constructioncon-marketplace-bff): from a multi-tenant monolith to a microservices proof of concept: Node.js catalog on MongoDB, [Spring Boot orders on SQL Server](https://github.com/J4kedi/constructioncon-marketplace-orders-svc), a BFF aggregator, a Next.js microfrontend and a serverless quote function on Azure Functions, orchestrated with Docker Compose and built by GitHub Actions.
- [Direct-mapped cache simulator](https://github.com/J4kedi/t1a-cache-mapeamento-direto-python): write-back and write-allocate, with unit tests and a step-by-step Tkinter view.
- [Process precedence graph](https://github.com/J4kedi/tde_performance): Python multiprocessing synchronized with semaphores, including a deadlock demonstration.

## Stack

**Languages and frameworks:** TypeScript, Node.js, NestJS, Next.js · Java 21, Spring Boot 3 · PHP

**Data and messaging:** PostgreSQL, MariaDB/MySQL, SQL Server, MongoDB · Redis, RabbitMQ, BullMQ · transactional outbox, expand/contract migrations

**Architecture:** modular monolith, microservices with BFF and microfrontend, schema-per-tenant and shared-schema multi-tenancy, queues with DLQ, idempotency, blue/green, edge failover

**Cloud and containers:** Docker and Compose, GHCR, GitHub Actions (hosted and self-hosted), Cloudflare (Workers, KV, R2, Tunnel), Fly.io, Neon, Linux, Nginx, Apache

**Quality and observability:** Jest, Playwright, k6, Prometheus, Grafana, Loki, Sentry/GlitchTip

## Contact

- Email: kauanpardini@gmail.com
- LinkedIn: [Kauan Pardini Augusto](https://www.linkedin.com/in/kauan-pardini-augusto-7b132b210/)
