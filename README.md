# Kauan Pardini Augusto

I'm a back-end and full stack developer in Curitiba, Brazil, in my final year of Software Engineering at PUCPR.
I'm looking for a junior or mid-level back-end or full stack role, remote or in Curitiba.

## Work

**Beauty Max Tecnologia**, Software Developer, since April 2026. A multi-tenant SaaS for beauty businesses (Next.js, PostgreSQL, Redis, RabbitMQ) and the group's internal support platform (GLPI, NestJS, MariaDB).

- Replaced the in-place PM2 deploy with blue/green CI/CD (staging, migration dry-run, health checks, smoke tests, rollback slot). 211 production releases in 11 weeks.
- Built the WhatsApp assistant that answers and books appointments with an LLM. It can book, cancel and reschedule, hands off to a person, has a kill switch, and only got write access after passing an 18-case offline eval.
- Built payment reconciliation against the provider's paid charges. It matches only by the invoice ID we write into the checkout metadata, never by amount, runs as a dry run by default, and sends anything ambiguous to a person.
- Made the per-operator cash register safe under concurrent use (row lock on opening, unique index that returns 409 instead of 500).
- Built WhatsApp message intake (webhook, RabbitMQ, worker) with message-ID dedupe in Redis plus a unique index, without dropping retries.
- Migrated the help desk to GLPI 11 in containers on MariaDB. A full rehearsal with matching row counts took about 4 minutes, against a 30-45 minute estimate.
- Built a WhatsApp-to-ticket service (NestJS, Kysely, transactional outbox, BullMQ) and PHP plugins that open expiring remote-access sessions from a ticket.
- Fixed silent edit loss in our self-hosted Docmost fork (Yjs): dropped frames now resync in about a second.

## Personal project: a construction management SaaS

I build and run a construction management SaaS in production. It's a modular monolith (NestJS and Next.js, 35 API modules) with shared-schema multi-tenancy, running as 32 Docker Compose services (production, staging and monitoring) on a VM I manage, behind Cloudflare. I moved it off Fly.io and kept Fly as a standby behind a circuit breaker in a Cloudflare Worker. The code is private, so these are the numbers I measured before and after each change:

| Change | Before | After | How |
| --- | ---: | ---: | --- |
| Dashboard p99 at 12 req/s | 3.3 s | 210 ms | Released the SSR payload right after render; the web tier had been restarting on heap under load |
| First request after an idle period | 110 ms | 20 ms | Kept the Postgres pool warm; the driver's 10 s idle timeout was draining it |
| Cached page at the edge | ~60 ms | 28-41 ms | Edge cache in a Cloudflare Worker, after normalizing the Next.js `Vary` header that was silently disabling the cache |
| Managed database compute | 1.99 CU-h/day | ~0.07 CU-h/day | Moved the primary to self-hosted Postgres with 5 days left on the quota; the managed copy stays for recovery |
| Restore from backup | - | 14-17 s | Hourly dumps to object storage, checked every month by an automated restore job |

Two decisions I like to explain:

- The failover at the edge only retries on the standby when the primary never answered. A request that changes data is never retried once the primary has replied. Promoting the standby is a script that refuses to run if the standby isn't on the current version.
- I keep a before and after record for every performance or cost change, including the ones that didn't help.

## Projects

- [Kurio](https://github.com/J4kedi/frontend-challenge) ([live demo](https://kurio-nft-marketplace-vert.vercel.app)): an NFT marketplace I built for a front-end challenge, in React 19, TypeScript, TanStack Router and Query, Tailwind CSS and shadcn/ui. The API and real-time events are mocked with MSW and Socket.IO. Playwright E2E and visual tests with axe, and Lighthouse audits.
- [CTAIO Locale Rescue](https://github.com/J4kedi/ctaio-locale-rescue) ([live page](https://j4kedi.github.io/ctaio-locale-rescue/)): I found that three of the six hreflang locales a public site declares answered 403. A Node script with no dependencies emits hreflang only for the locales that were actually built, and in the AI translation flow the model writes but a script decides what ships.
- [API-Rest-Java](https://github.com/J4kedi/API-Rest-Java): REST API in Java 21 and Spring Boot 3 with Spring Security/JWT, JPA, Flyway, OpenAPI and tests. A study project from a course.
- [ConstructionCon](https://github.com/J4kedi/constructioncon-marketplace-bff): from a multi-tenant monolith (Next.js, NextAuth, Prisma, PostgreSQL) to a microservices proof of concept: a Node.js catalog on MongoDB, [Spring Boot orders on SQL Server](https://github.com/J4kedi/constructioncon-marketplace-orders-svc), a BFF, a Next.js microfrontend and a quote service, run with Docker Compose and built by GitHub Actions.
- [Direct-mapped cache simulator](https://github.com/J4kedi/t1a-cache-mapeamento-direto-python): write-back and write-allocate, with unit tests and a step-by-step Tkinter view.
- [Process precedence graph](https://github.com/J4kedi/tde_performance): Python multiprocessing synchronized with semaphores, including a deadlock demo.

## Stack

- Languages and frameworks: TypeScript, Node.js, NestJS, Next.js, Java 21, Spring Boot 3, PHP
- Data and messaging: PostgreSQL, MariaDB/MySQL, SQL Server, MongoDB, Redis, RabbitMQ, BullMQ
- Architecture: modular monolith, microservices with a BFF and a microfrontend, multi-tenancy (schema per tenant and shared schema), queues with retries and backoff, transactional outbox, expand/contract migrations, idempotency, blue/green deploys, edge failover
- Infra: Docker and Compose, GHCR, GitHub Actions (hosted and self-hosted runners), Cloudflare (Workers, KV, R2, Tunnel), Fly.io, Neon, Linux, Nginx, Apache
- Testing and monitoring: Jest, Playwright, k6, Prometheus, Grafana, Loki, Sentry/GlitchTip

## Contact

- Email: kauanpardini@gmail.com
- LinkedIn: [Kauan Pardini Augusto](https://www.linkedin.com/in/kauan-pardini-augusto-7b132b210/)
