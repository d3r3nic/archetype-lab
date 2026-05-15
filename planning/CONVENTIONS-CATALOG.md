# Conventions Catalog v3 - Final Structure

1 meta-convention + 22 core conventions + 2 optional addons = 25 total

Sources: [A] User's existing CLAUDE.md files | [B] Traditional research (500+) | [C] AI-first research | [D] System prompt (excluded)

---

## #0 REUSABILITY & COMPOSITION (META-CONVENTION)

The convention that governs all conventions. Not a peer - sits above everything.

Everything built once, configured for context. When any convention is applied to a project, it produces ONE reusable foundational system. Features never build ad-hoc - they plug into systems.

Sources: [A] very strong (factory-first, config-driven, widget pattern) | [B] strong (composition, DRY, headless) | [C] strong (DRY enforcement through context engineering)

Includes: factory-first thinking, config-driven design, composition over configuration, widget pattern, single source of truth, one table/one button/one theme, headless patterns.

Overrides system prompt: "don't design for hypothetical futures" - in this framework, reusable systems are always the standard.

---

## FOUNDATION LAYER

### #1 PROJECT SETUP

How the project is structured, dependencies managed, and environment configured.

Sources: [A+B+C] all strong

Includes:
- Folder structure (feature-based, colocation, shared/ directory)
- File organization (small focused files, single responsibility, consistent structure)
- Path aliasing (@/ or ~/) and barrel exports
- Dependency management (evaluate before adding, wrappers, lockfiles, no --force)
- Environment configuration (.env hierarchy, prefixes, type-safe validation, no secrets in client)

Reusable system produced: project scaffold with established structure, path config, env validation.

### #2 GIT & VERSION CONTROL

How code moves from developer to production.

Sources: [B] strong (conventional commits, branching) | [C] strong (ultra-granular commits as save points)

Includes:
- Commit conventions (conventional commits or project standard)
- Branching strategy (trunk-based or gitflow)
- PR process and size limits
- Pre-commit hooks (lint, format, type-check)
- AI-era: ultra-granular commits as save points for rollback

---

## ARCHITECTURE LAYER

### #3 CODE ARCHITECTURE & PATTERNS

How modules and layers are separated. Which design patterns apply.

Sources: [A] strong (handler→service→db, feature isolation) | [B] strong (design patterns catalog)

Includes:
- Separation of concerns (logic/view/style)
- Feature isolation (no cross-feature imports)
- Handler → service → data access layering
- Design patterns: factory, adapter, facade, strategy, middleware
- Module boundaries with explicit public API
- Dependency injection via context/hooks
- No business logic in components

Reusable system produced: service layer architecture, shared utilities, module boundary enforcement.

### #4 COMPONENT DESIGN & API

How UI components are built and their interfaces designed.

Sources: [A] strong (atomic design, factory widgets) | [B] very strong (props, variants, compound, polymorphic)

Includes:
- Component hierarchy (atomic design or project pattern)
- Consistent API: size (xs-xl), variant (primary/ghost), state (isDisabled/isLoading)
- Props design (minimal surface, object grouping, discriminated unions)
- Controlled + uncontrolled support
- Compound component pattern
- Ref forwarding, spread props, polymorphic as prop

Reusable system produced: base component library, wrapper components around UI library, consistent API patterns.

### #5 STATE MANAGEMENT & DATA FLOW

How state is organized and flows through the app.

Sources: [A] strong (flat Redux, RTK Query) | [B] very strong (comprehensive)

Includes:
- Local-first state (useState by default)
- Server state vs client state separation
- Global state minimalism (auth, theme, locale only)
- Flat state tree, hierarchical component tree
- Unidirectional data flow
- URL as state for navigation/filters
- Normalized state shape

Reusable system produced: store configuration, state slice pattern, cache strategy.

---

## VISUAL LAYER

### #6 STYLING, THEMING & RESPONSIVE DESIGN

How visual design is implemented. Merged: styling + responsive + animation basics.

Sources: [A] strong (never hardcode colors) | [B] very strong (full CSS + tokens + responsive + motion)

Includes:
- CSS methodology choice (BEM, CSS Modules, CSS-in-JS, utility-first)
- Design token hierarchy (primitive → semantic → component)
- Theme object as single source of truth
- Never hardcode colors, spacing, shadows, typography
- Dark mode (data-theme + semantic tokens)
- Z-index scale (named constants)
- Spacing and typography scales
- Mobile-first responsive design
- Breakpoint scale (sm, md, lg, xl)
- Container queries for component-intrinsic responsiveness
- Touch target minimum 44x44px
- Motion tokens (duration, easing)
- Respect prefers-reduced-motion

Reusable system produced: theme system (tokens, scales, wrappers), responsive utilities, UI library wrappers.

---

## LANGUAGE LAYER

### #7 TYPE SAFETY & TYPE SYSTEM

How the type system prevents errors and documents code for AI.

Sources: [A] strong (no any, import type) | [B] very strong | [C] medium (types as AI documentation)

Includes:
- Strict mode in tsconfig
- No any (use unknown + type guards)
- Discriminated unions, branded types
- Runtime validation with Zod at boundaries
- Utility types (Partial, Pick, Omit, Record)
- Explicit return types on public functions
- Types as documentation for AI (richer types = better AI output)

Reusable system produced: shared type definitions, validation schema library, branded ID types.

### #8 ERROR HANDLING, RECOVERY & ASYNC

How errors are handled and async operations managed. Merged: errors + async.

Sources: [A] strong (centralized errors) | [B] very strong (boundaries, patterns, retry, fallback)

Includes:
- Error boundaries at app/route/feature levels
- Centralized error service (catch, log, report, retry)
- Result/Either pattern for pure functions
- Custom error classes (NetworkError, ValidationError)
- User-facing messages (friendly, actionable, no stack traces)
- Retry with exponential backoff for transient errors
- Fallback UI (skeleton, partial failure, offline, empty state)
- Async/await conventions (no .then chains)
- AbortController for cancellation
- Loading/error/success triads for every async op
- No floating promises

Reusable system produced: error service, error boundary components, error UI components (fallback, empty, offline), loading state components.

---

## DATA LAYER

### #9 API INTEGRATION & DATA FETCHING

How frontend communicates with backend. Includes real-time.

Sources: [A] strong (RTK Query, Network-First) | [B] very strong

Includes:
- Dedicated API layer (centralize all HTTP calls)
- Configured client instance (base URL, interceptors)
- Data fetching library for caching/dedup/revalidation
- Caching strategy (stale-while-revalidate or network-first)
- Optimistic updates
- Pagination patterns (offset, cursor, infinite scroll)
- Request/response transformation at boundary
- Real-time: WebSocket/SSE conventions, reconnection, state sync

Reusable system produced: API client, request/response interceptors, data fetching hooks, real-time connection manager.

### #10 FRONTEND-BACKEND CONTRACT

How frontend and backend agree on data shapes.

Sources: [A] medium (Zod, response envelope) | [B] strong (OpenAPI, tRPC, contract testing)

Includes:
- API contract as single source of truth (OpenAPI, GraphQL, tRPC)
- Generated TypeScript clients from contract
- Shared validation schemas (Zod)
- Contract testing
- Breaking change detection in CI
- Consistent response envelope

Reusable system produced: shared schema package, generated client, contract test setup.

### #11 AUTH & SECURITY

How authentication and security work across the stack.

Sources: [A] very strong (production lessons) | [B] strong (XSS, CSRF, CSP, tokens)

Includes:
- Use project's auth utility (never extract from JWT directly)
- Token storage in httpOnly cookies
- Access + refresh token pattern with silent refresh
- XSS prevention (no dangerouslySetInnerHTML, no eval)
- CSRF protection
- Content Security Policy
- Authorization at service layer
- Object access checks (verify user owns resource)
- Presigned URLs for file operations
- Never hardcode secrets
- Dependency security scanning
- Route protection for authenticated routes

Reusable system produced: auth service (token management, refresh, logout), auth context/provider, route guard component, security headers config.

---

## QUALITY LAYER

### #12 TESTING STRATEGY

How code is verified. Includes AI-era testing philosophy.

Sources: [B] very strong (methodology) | [C] very strong (TDD as highest-leverage AI practice)

Includes:
- Test behavior, not implementation
- TDD: write failing tests BEFORE AI implements
- Verification loops: run tests after every AI change
- Mock at boundaries only (MSW)
- Colocated test files
- Coverage thresholds on critical paths
- E2E for business-critical flows
- Visual regression testing
- Accessibility testing (jest-axe, keyboard)
- Tests as executable specification for AI

Reusable system produced: test utilities, custom render wrappers, mock factories, MSW handlers, test data factories.

### #13 PERFORMANCE & OPTIMIZATION

How speed and efficiency are maintained.

Sources: [B] very strong

Includes:
- Route-based code splitting
- Lazy loading (components, images, below-fold)
- Skeleton screens over spinners
- Bundle analysis and budgets
- Image optimization (modern formats, CDN, responsive)
- Font loading (swap, preload, subset)
- Core Web Vitals targets
- Virtual scrolling for long lists
- Content hashing for cache busting

Reusable system produced: lazy loading wrappers, image component, font loading config, performance monitoring setup.

### #14 ACCESSIBILITY

How the app works for everyone.

Sources: [B] very strong

Includes:
- Semantic HTML first (button for actions, a for navigation)
- ARIA patterns (no ARIA is better than bad ARIA)
- Keyboard navigation (tab order, arrow keys, escape to close)
- Focus management (trap in modals, return after close)
- Visible focus indicators
- Heading hierarchy
- Form accessibility (labels, aria-describedby for errors)
- Color contrast minimums (4.5:1 normal, 3:1 large)
- prefers-reduced-motion respect

Reusable system produced: accessible base components (Modal, Dialog, Dropdown built with proper ARIA), focus management utilities, skip link component.

### #15 BUILD, CI/CD & CODE QUALITY

How code ships and quality is enforced. Merged: build + code quality tooling.

Sources: [B] strong

Includes:
- ESLint strict + Prettier
- CI pipeline: install → lint → type-check → test → build → E2E → deploy
- Preview deployments per PR
- Feature flags (default off, cleanup after rollout)
- Rollback capability
- Dead code detection
- Tech debt tracking
- Dependency update automation
- Layered validation: linter → AI review → human review

Reusable system produced: CI pipeline config, linting config, formatting config, feature flag setup.

---

## KNOWLEDGE LAYER

### #16 DOCUMENTATION & DECISIONS

How decisions and usage are recorded. Includes AI-era documentation shifts.

Sources: [A] strong (feature READMEs) | [B] strong (ADRs, Storybook) | [C] strong (machine-parseable specs)

Includes:
- Comments explain WHY, not WHAT
- Feature README: what/why/how
- Architecture Decision Records (ADRs) for significant decisions
- Update documentation after code changes
- ARCHITECTURE.md for AI consumption
- Machine-parseable specifications (explicit, with examples)
- No over-commenting (AI over-comments 90-100% of the time)
- Structured TODO: // TODO(scope): description [ticket]
- Descriptive naming over brevity (calculateMonthlyRevenue not calcRev)
- Explicit over implicit (no magic strings, no convention-based wiring AI can't infer)

Reusable system produced: documentation templates, ADR template, README template.

---

## AI LAYER

### #17 CONTEXT MANAGEMENT

How code is structured for AI context windows.

Sources: [C] very strong

Includes:
- Tiered context: persistent (standards) → session (feature) → task (files)
- Lost-in-the-Middle awareness (critical info at start/end)
- Small focused files (<300 lines)
- Colocation of related logic, types, tests
- Proactive context compaction
- /clear between unrelated tasks
- Scoped instruction files (subdirectory CLAUDE.md)

### #18 VERIFICATION-DRIVEN DEVELOPMENT

How code is verified when AI writes it. The highest-leverage AI practice.

Sources: [C] very strong

Includes:
- TDD: write failing tests BEFORE AI implements
- Run tests after every AI change (not at the end)
- Build verification as hard gate (hooks block completion until passing)
- Screenshot verification for UI changes
- Tests as executable specification
- Verification is the bottleneck, not generation

### #19 AI STEERING & DRIFT PREVENTION

How humans steer AI and prevent drift. Merged: drift prevention + collaboration patterns.

Sources: [A] strong (behavioral rules) | [C] very strong (anti-drift patterns)

Includes:
- Specification-driven development (spec.md, plan.md, tasks.md)
- "Out of Scope" section prevents creep
- Plan-before-implement for multi-file changes
- Planning files that survive context resets
- Circuit breakers (warning + hard thresholds)
- Writer/reviewer split (two sessions)
- Session hygiene (fresh starts after 2 failed corrections)
- Bounded autonomy (approval gates per task type)
- Present options before implementing
- Stop and ask when uncertain
- Scope discipline (do exactly what was asked)
- Human retains: architecture, what NOT to build, comprehension
- Trust calibration over time (start tight, expand gradually)

---

## SPECIALIZED PATTERNS

### #20 FORMS & USER INPUT

How user input is collected and validated.

Sources: [B] strong

Includes:
- Form library for complex forms
- Schema-based validation (one schema = types + validation)
- Field-level validation on blur/change
- Multi-step wizard with per-step validation
- Dirty/pristine tracking
- Server validation errors mapped to fields
- Accessible error messages

Reusable system produced: form configuration, validation schema library, form field components, multi-step wizard framework.

### #21 ROUTING & NAVIGATION

How navigation works.

Sources: [B] strong

Includes:
- Nested routes/layouts for persistent UI
- Route guards for auth/authorization
- Loading/error UI per route
- URL state management (filters, pagination)
- Typed routes (prevent broken links)
- Route prefetching

Reusable system produced: route guard component, layout components, URL state utilities.

### #22 DESIGN SYSTEM

How a shared component library is built and maintained.

Sources: [A] medium (MUI wrappers) | [B] strong

Includes:
- Single source of truth component library
- Token-first design (tokens before components)
- UI library wrappers (never import directly from MUI/Chakra)
- Storybook as canonical catalog
- Versioned releases with changelogs
- Deprecation policy
- Visual regression testing on PRs

Reusable system produced: UI wrapper library, Storybook setup, visual testing config.

### #28 CONFIG-DRIVEN BRAND & CONTENT

How a template ships once and serves many customers without code edits per customer.

Sources: [A] strong (factory-first, config-driven, widget pattern — applied to content) | [D] downstream battle test (Step 60+: headless-wp-next template promoted from prototype to fork-ready when Edgar's content kept relying on stale template DEFAULTS instead of its own config)

Includes:
- Single typed schema for every brand-shaped value (branding, theme, nav, contact, social, content per page-shape, commerce, compliance, integrations, SEO)
- Three-layer resolver: env-var blob → on-disk fallback file → typed defaults
- Universal `getSiteConfig()` getter on server + client surfaces
- One canonical `docs/CONFIG.md` documenting every field
- Schema validation at trust boundaries (env parse + server-action input)
- "Edit JSON, never edit view code" as the customer-spawn contract
- Acceptable-hardcoding allowlist (state markers, framework infra, structural glyphs, page section ORDER)

Specifics that DO NOT belong in the framework convention (template-local per Rule #5b of factory CLAUDE.md):
- The specific env-var name (`NEXT_PUBLIC_SITE_CONFIG` for Next.js, `EXPO_PUBLIC_SITE_CONFIG` for Expo, etc.) — template decides
- The view-code syntax (`.tsx`, `.vue`, `.dart`) — template decides
- The validation library (Zod, Pydantic, ajv) — template decides

Reusable system produced: schema package + resolver + getter; config doc; defaults that render a neutral demo.

Relationship to #0: this is #0 (reusability) applied to BRAND & CONTENT specifically. Where #0 says "build it once, configure for context," #28 says "configure it from JSON, never from code."

---

## OPTIONAL ADDONS (included when project needs them)

### A1 INTERNATIONALIZATION

Sources: [B] strong

Includes: i18n library, key-based translation, namespace separation, RTL support (logical CSS properties), Intl API for formatting, locale detection/persistence.

### A2 OBSERVABILITY & MONITORING

Sources: [B] strong

Includes: RUM for Web Vitals, error tracking (Sentry), session replay, performance budgets, structured logging, alerting.

---

## SUMMARY

| Layer | Conventions | Count |
|-------|------------|-------|
| Meta | #0 Reusability | 1 |
| Foundation | #1-2 (Setup, Git) | 2 |
| Architecture | #3-5 (Architecture, Components, State) | 3 |
| Visual | #6 (Styling/Theming/Responsive) | 1 |
| Language | #7-8 (Types, Errors/Async) | 2 |
| Data | #9-11 (API, Contract, Auth/Security) | 3 |
| Quality | #12-15 (Testing, Performance, A11y, Build/CI) | 4 |
| Knowledge | #16 (Documentation) | 1 |
| AI | #17-19 (Context, Verification, Steering) | 3 |
| Specialized | #20-22 (Forms, Routing, Design System) | 3 |
| Optional | A1-A2 (i18n, Observability) | 2 |
| **Total** | | **25** |

## STATUS

v3 FINAL DRAFT - Merges applied, meta-convention elevated, ready for review.
