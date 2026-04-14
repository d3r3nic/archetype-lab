# Backend Conventions Audit

Audit of 37 backend-specific conventions against the 23 universal conventions.
Goal: identify what's genuinely new vs what overlaps.

## Overlaps with Universal (keep in universal, reference from backend)

| Backend Item | Universal Convention | Action |
|---|---|---|
| B2 boundary validation | #7 Types (runtime validation at boundaries) | Already covered. Backend adds: allowlist not blocklist, strip unknown fields. ADD these details to B2, REFERENCE #7. |
| B2 response envelope | #10 Contract (consistent response format) | Already covered. Backend adds: specific status codes, pagination metadata. ADD to B2, REFERENCE #10. |
| B6 storage abstraction | #0 Reusability (wrap third-party) + CLAUDE.md rule | Already covered as a general principle. Backend adds: specific StorageService interface. ADD to B6, REFERENCE #0. |
| B6 presigned URLs | #9 API (file upload section we added) | Partially covered from client side. Backend perspective is genuinely new. KEEP in B6. |

## Genuinely New (not in universal at all)

| Backend Convention | Why it's new |
|---|---|
| B1 ALL 10 database conventions | Universal has zero database coverage. Entirely new. |
| B2 URL design, HTTP methods, status codes, pagination, versioning | Universal #9 is about CONSUMING APIs. Building APIs is different. |
| B3 ALL 3 middleware conventions | Universal has zero middleware coverage. |
| B4 ALL 6 logging conventions | Universal #8 covers error handling but not logging/observability. |
| B5 ALL 4 background job conventions | Universal has zero async job coverage. |
| B7 ALL 4 caching conventions | Universal has zero server-side caching coverage. |

## What's Redundant (already fully covered, don't duplicate)

| Backend Item | Covered by | Action |
|---|---|---|
| "Never hardcode secrets" | CLAUDE.md rule + #1 Project Setup + #11 Auth | DO NOT duplicate in backend conventions |
| "Use centralized error handling" | CLAUDE.md rule + #8 Errors | DO NOT duplicate. Backend convention can REFERENCE. |
| "Feature isolation" | #3 Architecture | DO NOT duplicate. |
| "Test after every change" | #18 Verification | DO NOT duplicate. |

## Architecture Decision: How FE/BE/Universal Connect

```
project/
├── CLAUDE.md                     ← universal enforcer (at root, auto-loaded)
│   References both universal AND endpoint-specific conventions
│   Has a section: "If this is a backend project, also read backend/CLAUDE.md"
│   Has a section: "If this is a frontend project, also read frontend/CLAUDE.md"
│
├── archetype/
│   ├── Conventions.md            ← universal lookup index (25 conventions)
│   ├── conventions/              ← universal convention docs (25 files)
│   │
│   ├── backend/                  ← BACKEND-SPECIFIC
│   │   ├── CLAUDE.md             ← backend enforcement rules (supplements universal)
│   │   │   "Never run destructive migrations without permission"
│   │   │   "Never create connections per request"
│   │   │   "Never log sensitive data"
│   │   │   etc.
│   │   ├── Conventions.md        ← backend convention index (7 conventions)
│   │   └── conventions/          ← backend convention docs
│   │       ├── B1-database.md
│   │       ├── B2-api-design.md
│   │       ├── B3-middleware.md
│   │       ├── B4-logging.md
│   │       ├── B5-background-jobs.md
│   │       ├── B6-file-handling.md
│   │       └── B7-caching.md
│   │
│   ├── frontend/                 ← FRONTEND-SPECIFIC (future)
│   │   ├── CLAUDE.md             ← frontend enforcement rules
│   │   ├── Conventions.md        ← frontend convention index
│   │   └── conventions/          ← frontend convention docs
│   │       ├── F1-component-design.md (currently universal #4)
│   │       ├── F2-state-management.md (currently universal #5)
│   │       ├── F3-styling.md (currently universal #6)
│   │       ├── F4-accessibility.md (currently universal #14)
│   │       ├── F5-forms.md (currently universal #20)
│   │       ├── F6-routing.md (currently universal #21)
│   │       └── F7-design-system.md (currently universal #22)
│   │
│   └── (everything else stays as is)
```

## AI Reference Flow

```
AI starts session on a BACKEND project:
  1. Reads root CLAUDE.md (universal enforcer, 19 rules)
  2. Root CLAUDE.md says: "This is a backend project. Also read archetype/backend/CLAUDE.md"
  3. Reads archetype/backend/CLAUDE.md (backend-specific enforcement)
  4. Scans archetype/Conventions.md (universal index - reads only relevant universal conventions)
  5. Scans archetype/backend/Conventions.md (backend index - reads relevant backend conventions)
  6. Reads References.md (project context)
  7. Now the AI has: universal rules + backend rules + project context

AI starts session on a FRONTEND project:
  1. Reads root CLAUDE.md (universal enforcer)
  2. Root CLAUDE.md says: "This is a frontend project. Also read archetype/frontend/CLAUDE.md"
  3. Reads frontend enforcement
  4. Scans both indexes (universal + frontend)
  5. Reads References.md
  6. Now has: universal rules + frontend rules + project context

AI starts session on a FULLSTACK project:
  1. Reads root CLAUDE.md
  2. Working on backend code? Read backend/CLAUDE.md + backend conventions
  3. Working on frontend code? Read frontend/CLAUDE.md + frontend conventions
  4. Working on shared code? Universal conventions only
```

## Key Design Decisions

1. Universal conventions STAY universal. They are not duplicated in backend/ or frontend/.
2. Backend conventions REFERENCE universal ones ("see also #7 for type validation at boundaries").
3. Backend CLAUDE.md SUPPLEMENTS the universal enforcer, not replaces it. Both are read.
4. The root CLAUDE.md determines which endpoint-specific CLAUDE.md to also read based on project type (References.md says whether this is frontend, backend, or fullstack).
5. Frontend-specific conventions are a FUTURE step. Currently #4, #5, #6, #14, #20, #21, #22 sit in universal even though they're frontend-only. Moving them to frontend/ would be a breaking restructure - defer for now.
6. Backend conventions follow the same doc template as universal: Principle, Reusable System, Rules, Violations, Wrong vs Right, Research Notes.

## Backend CLAUDE.md Enforcement Rules (draft)

These supplement the 19 universal rules:

- Never run destructive database operations (DROP, DELETE, ALTER) without explicit approval. → backend/conventions/B1-database.md
- Never create database connections per request. Use the connection pool. → backend/conventions/B1-database.md
- Never fetch inside a loop. Use batch queries, joins, or includes. → backend/conventions/B1-database.md
- Never put business logic in handlers. Use the service layer. → universal #3 (architecture)
- Never use console.log. Use the structured logging system. → backend/conventions/B4-logging.md
- Never log passwords, tokens, API keys, or PII. → backend/conventions/B4-logging.md
- Never do synchronous work that takes over 2 seconds in a request handler. Queue it. → backend/conventions/B5-background-jobs.md
- Never trust client Content-Type headers for file validation. Check magic bytes. → backend/conventions/B6-file-handling.md
- Never return raw arrays from API endpoints. Use the response envelope. → backend/conventions/B2-api-design.md

## Next Steps

1. Write the 7 backend convention docs (B1-B7) in the factory first
2. Write backend/CLAUDE.md with the enforcement rules above
3. Write backend/Conventions.md as the lookup index
4. Update root CLAUDE.md to reference endpoint-specific CLAUDE.md
5. Test on Development3/backend
6. FUTURE: move frontend-only conventions (#4, #5, #6, #14, #20, #21, #22) to frontend/ folder
