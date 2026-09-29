# Kauan Pardini Augusto

Software engineer, backend and full stack. I build and operate production systems, and I measure what changes.
Curitiba, Brazil · final-year Software Engineering student at PUCPR · Java teaching assistant.

## Currently

**Beauty Max Tecnologia**, Software Developer (Apr 2026 – present). Multi-tenant SaaS for beauty businesses: Next.js, PostgreSQL, Redis, RabbitMQ.

- Designed the blue/green CI/CD the team ships with: 211 production releases in 11 weeks, a median of 20 minutes from merge to production, previous slot kept for rollback.
- Built invoice reconciliation against the payment provider: matches charges by checkout metadata, runs dry by default, is safe to re-run, and sends anything it cannot correlate to human review instead of guessing by amount.
- Made per-operator cash registers safe under concurrent use (row lock on opening, unique index returning 409 instead of 500).
- Built WhatsApp inbound processing (webhook → RabbitMQ → worker) with message-ID dedupe in Redis plus a unique index, without dropping retries.

## ConstruXion, my own SaaS project

A construction-management SaaS in production at [construxion.com.br](https://www.construxion.com.br): NestJS, Next.js, PostgreSQL, Redis, RabbitMQ, self-hosted on Linux behind Cloudflare. The code is private, so here are the results, each measured before and after the change.

| Change | Before | After | How |
| --- | ---: | ---: | --- |
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
- [ConstructionCon Marketplace](https://github.com/J4kedi/constructioncon-marketplace-bff): architecture proof of concept, a monolith evolving into a BFF plus separate [orders (Java)](https://github.com/J4kedi/constructioncon-marketplace-orders-svc) and catalog (TypeScript) services, with a microfrontend and Docker Compose.
- [Direct-mapped cache simulator](https://github.com/J4kedi/t1a-cache-mapeamento-direto-python): write-back and write-allocate, with unit tests and a step-by-step Tkinter view.
- [Process precedence graph](https://github.com/J4kedi/tde_performance): Python multiprocessing synchronized with semaphores, including a deadlock demonstration.

## Stack

TypeScript, Node.js, NestJS, Next.js · Java 21, Spring Boot 3 · PostgreSQL, Redis, RabbitMQ, BullMQ · Docker, GitHub Actions, Nginx, Linux, Cloudflare Workers · Jest, Playwright · Prometheus, Grafana, Loki, Sentry

## Contact

- Email: kauanpardini@gmail.com
- LinkedIn: [Kauan Pardini Augusto](https://www.linkedin.com/in/kauan-pardini-augusto-7b132b210/)
