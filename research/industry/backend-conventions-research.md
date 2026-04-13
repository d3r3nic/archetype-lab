# Backend-Specific Conventions Research

Date: 2026-04-13
Source: Web research across CodeRabbit, Stack Overflow, boringSQL, Augment Code, CSA, and 20+ other sources.

## 7 Categories Entirely or Mostly Missing from Universal Framework

### B1: Database (10 conventions)
- N+1 query prevention (AI violation: EXTREMELY HIGH, 8x more I/O in AI code)
- Select only needed fields, never SELECT * (AI: HIGH)
- Migration safety: never auto-run destructive, backup first, separate destructive from additive (AI: HIGH)
- Index design based on actual queries not guesses, composite indexes, partial indexes (AI: HIGH)
- Database constraints as safety nets (NOT NULL, UNIQUE, CHECK, FK) (AI: MEDIUM)
- Soft delete discipline: every query must filter deleted_at IS NULL (AI: HIGH)
- Transaction discipline: wrap related writes, never leave open (AI: VERY HIGH)
- Connection pooling: one pool at startup, never per-request (AI: VERY HIGH)
- Seeding discipline: separate reference from dev data, never seed prod with dev data (AI: MEDIUM)
- ORM anti-patterns: review generated SQL, watch for lazy loading traps (AI: VERY HIGH)

### B2: API Design - building APIs (7 conventions)
- Resource-based URL design: nouns not verbs, plural, lowercase with hyphens (AI: MEDIUM)
- HTTP method semantics: GET read, POST create, PUT replace, PATCH partial, DELETE remove (AI: LOW-MEDIUM)
- Status code consistency: same codes for same situations across all endpoints (AI: MEDIUM)
- Pagination: offset vs cursor, always return metadata, default+max limits (AI: HIGH)
- API versioning from day one (AI: LOW - missing rather than wrong)
- Request/response validation at boundary: allowlist not blocklist (AI: VERY HIGH, 45% of AI code has security vulns)
- Consistent response envelope across all endpoints (AI: MEDIUM)

### B3: Middleware/Pipeline (3 conventions)
- Middleware ordering: error handler → request ID → logging → security → rate limit → auth → authz → validation → handler (AI: MEDIUM)
- Request lifecycle as a pipeline not free-for-all (AI: HIGH)
- Cross-cutting concerns: correlation IDs, timing, never log request bodies (AI: HIGH)

### B4: Logging & Observability (6 conventions)
- Structured logging: JSON format, proper library, never console.log (AI: VERY HIGH)
- Log level discipline: ERROR for broken, WARN for unexpected, INFO for operational (AI: MEDIUM)
- Never log sensitive data: passwords, tokens, PII (AI: VERY HIGH)
- Correlation IDs: generate at edge, propagate everywhere (AI: VERY HIGH)
- Health checks: liveness (process alive) + readiness (deps connected) (AI: HIGH)
- Metrics: RED method (rate, errors, duration) for every service (AI: HIGH)

### B5: Background Jobs (4 conventions)
- Async vs sync decision: queue anything over 1-2 seconds or with external deps (AI: HIGH)
- Idempotency: every job handler must handle duplicate execution (AI: VERY HIGH)
- Retry with exponential backoff + jitter + max retries + dead letter queue (AI: HIGH)
- Job design: small payloads, separate processes, TTLs, rate limits (AI: HIGH)

### B6: File Handling - server side (3 conventions)
- Presigned URLs: server generates URL, client uploads directly to storage (AI: HIGH)
- Server-side validation: magic bytes not Content-Type header, malware scan (AI: HIGH)
- Storage abstraction: wrap behind interface, never SDK in feature code (AI: MEDIUM)

### B7: Caching - server side (4 conventions)
- When to cache: read-heavy + infrequent changes, never real-time data (AI: MEDIUM)
- Cache invalidation: TTL-based is simplest, event-driven for consistency (AI: LOW)
- Cache layers: in-memory, distributed (Redis), CDN (AI: LOW)
- Cache stampede prevention: per-key locking or stale-while-revalidate (AI: HIGH)

## AI Anti-Pattern Rankings

VERY HIGH frequency (AI almost always gets wrong):
- Missing transactions, connection pool misuse, console.log everywhere, missing input validation, no correlation IDs, no idempotency, logs sensitive data, N+1 queries, trusts ORM defaults

HIGH frequency:
- SELECT *, dangerous migrations, wrong indexes, soft delete leaks, no pagination, everything synchronous, naive retries, file proxying, trusts Content-Type, no health checks, auth in handler, cache stampede, no metrics

## Statistics
- AI-authored PRs: 10.83 issues/PR vs 6.45 human (1.7x more)
- 8x more excessive I/O in AI code
- 45% of AI code has security vulnerabilities
- 74 CVEs attributed to AI code (as of March 2026)
