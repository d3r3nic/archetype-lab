# Framework Changelog

Every improvement to the Archetype framework, why it was made, and what triggered it.

## 2026-04-17 (Step 39) — Phase 4 (Maintain) first audit + structural parity fix

Trigger: first-ever Phase 4 agent test against real game-test project (now 2 features, 13/13 tests). Agent produced 9 tech-debt entries, session review, and feature-tree Audit Log row. Surfaced a CRITICAL structural defect: MAINTAIN.md's inline audit script and `validate-develop.sh` gave DIFFERENT answers on the same repo. Phase 4 structural asymmetry confirmed:
- Phase 1: 4 routed docs + validator + hooks
- Phase 2: 6 routed docs + validator
- Phase 3: 2 routed docs + validator
- Phase 4: 1 doc only, no validator, no RED-FLAGS, no templates parity

Agent verdict: "Phase 4 is under-specified by roughly the same margin DEVELOP.md was before Step 37."

Changes (full parity pass):

1. **`templates/technical-debt.md` (NEW).** Parity with `session-review.md` and `feature-doc-template.md`. Categorical format for tech-debt log entries (id, date logged, what, where, convention, severity, proposed fix, status). Framework-agnostic — no language or tool specifics.

2. **`development/MAINTAIN-RED-FLAGS.md` (NEW).** Parity with the 3 existing RED-FLAGS docs. 5 Phase 4 silent-failure patterns from agent:
   - Audit done once then forgotten (no trigger discipline)
   - Tech-debt log grows forever without pruning (no aging / escalation rule)
   - Convention evolution captured in session review but never promoted (no feedback loop)
   - Doc freshness drift accumulates invisibly (hooks "handle it" = defense-in-name-only)
   - Two audits disagreeing on the same repo (MAINTAIN inline bash vs validate-develop.sh)

3. **`scripts/validate-maintain.sh` (NEW).** Machine-verifiable Phase 4 gates:
   - TECHNICAL-DEBT.md exists at project root (or justified absence documented)
   - `feature-tree.md` has an Audit Log section with at least one entry if features/ has > N features
   - `docs/reviews/` has at least one review after N sessions (heuristic — or justified absence)
   - Features/ directory basename == feature-tree.md Feature column == docs/features filename (the TD-001 fix)
   - No entries in TECHNICAL-DEBT.md with Status=Open older than N cycles without severity escalation

4. **`development/MAINTAIN.md` rewritten** (177 → ~280 lines, operational):
   - Trigger table at top: routine audit (every N commits or 2-4 weeks), incident response (after a failed deploy / flaky test / convention violation discovery), convention evolution (after accumulated session reviews).
   - Inline bash audit script REMOVED — routed to `scripts/validate-maintain.sh` and `scripts/validate-develop.sh`. No more two-audits-disagreeing problem.
   - Per-dimension operational checklist (feature-tree audit, doc freshness, convention violations, tech-debt pruning).
   - Tech-debt pruning rule: entries Open > N cycles either escalate severity or get force-fixed as part of the next feature touching that area.
   - Convention evolution flow: session-review finding → framework issue → PR.

5. **CLAUDE.md: feature-directory naming rule.** Added: "Feature directory basename, feature-tree.md Feature column, and docs/features filename must all match. If business identity differs from directory name, rename so they align."

6. **`scripts/validate-develop.sh` updated.** Previous version exempted `health|_health|ping|smoke` as a hardcoded name allowlist. Agent flagged this as fragile (TD-004). Fixed: exemption now driven by a `status: smoke-test` value in feature-tree.md rather than directory name.

7. **References.md template updated.** Added "Convention Overrides → Test isolation strategy" stub so Phase 3 agents can fill it at scaffold time. Prevents TD-003 (test isolation choice undocumented).

Zero expirable content. All new artifacts categorical.

**Framework status after Step 39:**
- Phase 1: ✅ converged (4 rounds of audits)
- Phase 2: ✅ converged (3 rounds)
- Phase 3: ✅ converged (2 rounds)
- Phase 4: structural parity now achieved; v2 verification needed

Next: run Phase 4 v2 against game-test to verify fixes converge.

## 2026-04-17 (Step 38) — Phase 3 convergence (v2) + two narrow silent-failure fixes

Trigger: Phase 3 agent v2 against Step 37 framework. Converged cleanly — all 4 targeted fixes (Conventions.md routing, DEVELOP.md inventory + convention-routing + minimum-tests) worked as designed. Test count 7→13, validator 0 errors first try, RED-FLAGS #2 held under type-system pressure. Agent surfaced 2 residual silent-failure patterns (narrower than v1's):

1. **Cross-feature test-data contamination** when integration tests share a DB. Sibling test files leave rows; list-endpoint tests asserting total counts flake. Not covered by any doc. Agent hit it, diagnosed quickly, fixed by rewriting assertions to be isolation-resilient — but nothing would have warned a less-careful agent.

2. **Type-system escape-hatch temptation.** Prisma's optional-field generics fought `exactOptionalPropertyTypes`. The tempting fixes (`as any`, `new PrismaClient({...})` with different config) violate conventions silently. Agent held the line but it was narrow.

Fixes:
- **development/RED-FLAGS.md #8 (NEW):** test-isolation strategies when integration tests share state — set-membership with test-scoped prefix, transaction rollback per test, separate DATABASE_URL per test file, testcontainers with reset. Categorical, not prescriptive.
- **development/RED-FLAGS.md #9 (NEW):** escape-hatch on type errors. Never `as any`, `@ts-ignore`, or re-instantiate a shared class with laxer config. Refactor the call site.
- **DEVELOP.md Step 5:** inline callout on test isolation with minimum bar (scoped lookups, not total-count assertions).
- **CLAUDE.md rule:** "Never escape-hatch a type error with casts, ignore-comments, or re-instantiating a shared class with laxer config. Refactor the call site."

Zero new expirable content. All additions categorical patterns with research-at-scaffold guidance.

**Phase 3 convergence.** After 2 rounds of agent audits against REAL code, Phase 3 is production-ready. Remaining polish items (feature-doc drift automation, B2 envelope validator checks) are nice-to-have, not silent-failure.

**Framework phase status:**
- Phase 1 (Bootstrap): ✅ converged across 4 rounds + 6 surface tests
- Phase 2 (Scaffold): ✅ converged across 3 rounds
- Phase 3 (Develop): ✅ converged across 2 rounds
- Phase 4 (Maintain): 🔴 still untested — the last surface

Next: test Phase 4 against game-test (now has 2 real features, 13 passing tests, real tech debt candidates).

## 2026-04-17 (Step 37) — Phase 3 (Develop) first audit + fix

Trigger: first-ever Phase 3 agent test against a REAL working scaffolded project (game-test: Node 24 + Fastify 5 + Prisma 6 + SQLite, all 8 foundational systems built, 2/2 existing tests passing). Agent added `record-session` feature end-to-end: 4 files created, 2 modified, all 3 verification gates green (typecheck, 7/7 tests, build).

**What worked:** 100% foundational-system reuse (no rebuilds). Lazy-loading (2 of 33 convention docs read). Scaffold quality carried Phase 3 — health.test.ts was reference implementation for test placement.

**Critical silent-failure found:** Conventions.md "API / data fetching" row points backend users to #9 + #10 + #8 (client-side conventions). Agent had to fall through to backend/Conventions.md manually. Less-thorough agent would silently use client-side routing for a server endpoint.

**DEVELOP.md was thin (99 lines).** Agent got through because CLAUDE.md + scaffold reference code + task prompt crutched the gaps. DEVELOP.md alone would have allowed: rebuild shared systems, zero tests, skip feature doc, skip References.md update, partial commits.

Changes:

1. **Conventions.md routing fixed.** The "API / data fetching" row was wrong for backend users. Split into two rows: "API consumption (client-side)" → #9, #10 + #7, and "New API endpoint (server-side)" → backend/B2, #7, #8 (+ "also see backend/Conventions.md"). Stops silent wrong-routing for backend endpoint tasks.

2. **DEVELOP.md rewritten with operational gates.** 99 → ~200 lines, focused on what the agent MUST do not what it should know:
   - Step 1 expanded: "System inventory" gate — enumerate every shared system in feature-tree.md, map to "will use / not needed" before writing code.
   - Step 2 expanded: explicit convention-routing step — open Conventions.md, identify 3-5 relevant docs, STATE them before code.
   - Step 4 minimum-test rule added: for new HTTP endpoints requiring auth, the baseline is at least 4 integration tests (auth-fail, validation-fail, happy-path, edge-case like not-found/conflict). Categorical, not prescriptive.
   - Step 5 feature-doc template strengthened: added API shape (request/response/errors), error table, test coverage, key decisions as required H2s.
   - Step 6 commit-granularity rule: commit trigger is "a verification gate just passed." Never commit with failing tests.
   - Step 7 (new): Update References.md when new top-level paths emerge (docs/features/, new feature subfolders).

3. **CLAUDE.md: "never instantiate shared getter classes" rule.** Added: "If `src/shared/*` exports a getter (getDb, getLogger, etc.), never construct the underlying class directly. Type imports and namespace imports are OK." Prevents the "I'll just new up my own PrismaClient" drift — agent flagged this temptation during the test.

4. **development/RED-FLAGS.md (NEW).** Parallels bootstrap/RED-FLAGS.md and scaffolding/RED-FLAGS.md. 7 Phase 3 silent-failure patterns:
   - Rebuild shared systems (didn't read feature-tree inventory)
   - Bypass shared getter to instantiate class directly (framework #0 violation)
   - Zero tests (DEVELOP.md alone allows this)
   - Skip feature doc (buried at step 5)
   - Skip References.md update (wasn't instructed)
   - Partial commits (no granularity rule)
   - Route file and feature doc drift apart (no cross-link)

5. **scripts/validate-develop.sh (NEW).** 5 machine-verifiable gates per feature:
   - No `new PrismaClient()` / shared-class instantiation outside src/shared/
   - No `console.log` in src/features/ (use shared logger)
   - Every src/features/{name}/ has a test file matching {name}.test.ts
   - Every feature in feature-tree.md has a docs/features/{name}.md
   - No `throw new Error(` in features (use AppError subclasses)

Zero expirable content. All additions categorical or machine-gates.

Deferred:
- Phase 4 (Maintain) — still the one untested surface
- Feature-doc ↔ route cross-link (convention, but not silent-failure)
- Test placement convention (unit vs integration — project-specific)

## 2026-04-17 (Step 36) — Scaffold convergence (v3) + three surgical fixes

Trigger: scaffold agent v3 converged. 20/20 systems built with framework guidance (v1: 14, v2: 19, v3: 20). Zero silently-skipped systems. Zero red flags fired during build. Validator passed first try with zero errors and zero warnings across all 10 groups. 7 residual items found, all narrow polish.

Convergence signal: the scaffold phase is production-ready for backend projects. Frontend / mobile / platform variants are playbook-parallel; similar convergence expected.

Three fixes shipped (silent-failure risks only):

1. **validate-scaffold.sh CI-migration regex tightened.** v3 found the validator matched `workflow_dispatch` in COMMENTS (not just as a YAML `on:` trigger). Real false-positive: a config with `# old approach used workflow_dispatch` + actual `on: push: branches: [main]` would silently pass. Fixed: regex now anchors to YAML trigger position — `workflow_dispatch:` at start of logical line OR under an `on:` key, not as a free-form word.

2. **In-memory audit-store pre-production gate.** v3 flagged that an agent could ship an `InMemoryAuditStore` (test scaffold) to production silently. Audit log that disappears at process restart = SOC 2 compliance failure. Added validator check: if `audit-log` source references an in-memory implementation without a real backing store also present, WARN (because a dev-only pattern is valid early on but a production-deploy-blocker later). Escalates to FAIL if `References.md` compliance section names HIPAA/SOC2/PCI — at that point the in-memory store is incompatible with shipped code.

3. **DataLoader request-scope warning.** v3 built DataLoaders correctly from common knowledge but noted the playbook didn't EMPHASIZE that loaders must be request-scoped. Module-scope DataLoader = cross-request cache leak (security bug in multi-tenant systems). Added one-line warning in SCAFFOLD-BACKEND Step 12 GraphQL: "DataLoaders MUST be per-request instances (created inside the request context). Module-scope DataLoaders leak cached data across tenants — a security bug."

Not shipped (deferred as "compress don't delete" but not silent-failure):
- Contract testing concretization (categorical guidance already present via B2 versioning pointer)
- Migration-safety linter recommendation (over-prescriptive — squawk vs alternatives varies per DB)
- Secret scanning in pre-commit defaults (over-prescriptive — gitleaks vs trufflehog vs alternatives)
- OTel auto-instrumentation-register-only validator variant (rare edge case, warn not fail)

Next untested surfaces: Phase 3 (Develop) — feature-building on a scaffolded project. Phase 4 (Maintain) — audit + evolution.

## 2026-04-17 (Step 35) — Scaffold round 2 findings fix

Trigger: scaffold agent v2 (same backend GraphQL profile) ran against Step 34. 19/20 systems built with explicit framework guidance (vs 14/20 in v1), 0 silently skipped (vs 6), validator passed first run, 5 red flags caught + resolved. Six residual gaps surfaced — all narrower and more technical than v1's "entire systems missing" findings.

Fixes:

1. **validate-scaffold.sh CI-migration check rewritten.** v2's validator was brittle: treated safe `workflow_dispatch` manual-gate as same severity as unsafe auto-migrate-on-push, and double-counted the same file as both WARN and OK. Fixed: `workflow_dispatch` and staging/dev `if:` gates treated as safe; auto-migrate-on-`push: main/master` promoted from WARN to FAIL. No more false-positive/false-negative symmetry.

2. **Pre-commit hook validator check added.** `.husky/pre-commit` (or language equivalent: lefthook, pre-commit.yml, etc.) must exist OR validator fails. Missing pre-commit = silently-shipping-without-discipline, which is how console.log / unvalidated env / missing type checks end up in production.

3. **Persisted-queries validator check added.** If References.md mentions mobile clients OR public-client GraphQL, validator greps for `persistedQuer|persistedDocument` in the GraphQL setup. Absent = FAIL. Mobile clients with arbitrary-query execution is a production attack surface.

4. **OpenTelemetry exporter validator check added.** If References.md names OpenTelemetry/OTLP, validator requires either an SDK startup call or OTLP exporter config. Prom-client-only with OTLP env vars defined-but-unused = FAIL.

5. **Preamble deduplication.** The 8 shared scaffold rules (zero-stale, convention-mapping, handoff-check, smoke-test, verification-discipline, commit-discipline, red-flags, machine-verifiable-gate) were duplicated across SCAFFOLD.md + 4 playbooks. Extracted to `scaffolding/_preamble.md` — single source of truth. Each playbook includes a one-line reference. No more drift risk.

6. **DataLoader categorical mention added.** SCAFFOLD-BACKEND Step 12 GraphQL branch now explicitly routes to the DataLoader pattern (N+1 prevention). Categorical: "build a request-scoped loader per parent→children relationship used in resolvers." Does not name a specific library; research current options for the language.

Meta-finding: v2 identified zero structural gaps (no systems were silently skipped, no red flags missed). All 6 fixes are validator polish and one categorical addition. The framework's scaffold phase is converging.

Deferred:
- Rate-limit research-at-scaffold forcing function (advisory only — rate limits legitimately vary per project; forcing research may be over-prescriptive)
- Phase 3 (Develop) and Phase 4 (Maintain) — the next untested surfaces

## 2026-04-17 (Step 34) — Scaffold phase: first audit and fix

Trigger: first-ever Phase 2 (Scaffold) agent test against a realistic backend GraphQL profile (72 files generated). All 20 foundational systems built, but 6 of them were "improvised" — no SCAFFOLD.md step owned them. Agent identified 10 concrete silent-failure risks.

Core finding: Phase 2 is HIGHER-STAKES than Phase 1 (produces production code, not docs) but has LESS scaffolding (231 lines vs 474+3-routed-docs). Inverted investment.

Key gaps agent found:
- SCAFFOLD.md is ~60% frontend-shaped (Theme, Routing, State, Components, Forms = 5 of 13 steps). Backend agents improvise 13 of 20 systems.
- No steps for: middleware pipeline (B3), audit log SEPARATION from app log (#23+B4), rate limiting, idempotency (mutation-level), metrics, tenant isolation (the #1 B2B multi-tenant bug).
- Verification is aspirational: "Verify it works" with no exit criteria, no command map. Careless agent ticks checklist without running anything.
- No convention mapping per step. Backend B-conventions never named in step list.
- Top silent-failure risks: skipped-by-interpretation ("[backend]" tag not strong), audit log conflated into app log (SOC 2 failure), middleware order wrong, tenant isolation in schema but not queries, env validation at runtime not startup, GraphQL persisted-queries forgotten, migrations auto-run in CI (B1 violation).

Changes:

**1. SCAFFOLD.md split by project shape (parallel to references-*.md pattern).**
- `scaffolding/SCAFFOLD.md` becomes a router with shared preamble (read References.md compliance section first, general rules, verification philosophy, convention-mapping rule) + routing to shape-specific playbooks.
- `scaffolding/SCAFFOLD-FRONTEND.md` (new) — frontend order: project setup, types, errors, theme, components/design-system, state, routing, forms, API client, testing, CI.
- `scaffolding/SCAFFOLD-BACKEND.md` (new) — backend order: env validation, types, errors, app-log + audit-log (SEPARATE, explicit), db, cache, auth, authz, API server (REST or GraphQL branch), middleware pipeline (B3 order), rate limiting, idempotency, jobs, metrics, tenant isolation, testing, CI.
- `scaffolding/SCAFFOLD-MOBILE.md` (new) — mobile order: native-module wrapping, offline-sync, push, code-signing, TestFlight/Play Console.
- `scaffolding/SCAFFOLD-PLATFORM.md` (new) — platform-choice projects have no code scaffolding; they still need the non-code systems from the platform's config checklist.

**2. Each scaffold doc names conventions inline.** Every step says "build per convention #N (path)". Agent no longer has to infer mappings.

**3. Operational verification gates.**
- Each step ends with "Verify: [specific command, specific exit criterion]" — not "verify it works".
- `scripts/validate-scaffold.sh` (new) — machine-verifiable scaffold check. Gates: every system in feature-tree.md has a docs/systems/{name}.md, every foundational system path exists, specific anti-patterns absent (console.log in src/, raw axios/fetch imports in features, direct Prisma imports outside db wrapper, untyped escape hatches with no comment), env validation runs at startup (`loadEnv()` or equivalent called before server start).

**4. scaffolding/RED-FLAGS.md (new).** Parallels bootstrap/RED-FLAGS.md. 10 known silent-failure patterns from agent #1:
- Skipped-by-interpretation (frontend/backend tag not strong enough)
- Checklist verification without execution
- Audit log conflated into app log
- Middleware pipeline order wrong
- Rate limiting / idempotency forgotten
- Tenant isolation in schema but not queries (CRITICAL for multi-tenant B2B)
- Env validation at runtime not startup
- Frontend step titles applied to backend
- GraphQL persisted queries forgotten
- Migrations auto-run in CI

**5. "Smoke-test feature" requirement.** Agent #1 finding: scaffold can be "complete" with misconfigured wiring and nobody finds out until first real feature. New rule: scaffold ends with a minimal "health" or "ping" feature that uses every shared system, providing end-to-end integration proof.

**6. References.md-to-scaffold handoff check.** First step of scaffold: read References.md compliance and foundational-systems sections; inventory every system before building. Missed systems = skipped systems.

Zero new expirable content. All additions are categorical or routing pointers.

Deferred: Phase 3 (Develop) and Phase 4 (Maintain) — next untested surfaces.

## 2026-04-17 (Step 33) — Steering restructure: routing over content

Trigger: user audit of framework direction. Previous steps conflated minimalism with steering — proposed deletion when the correct move is compression + routing. Core principles realigned:

- **Goal is steering, not minimalism.** Large framework is fine if most docs are lookup targets the index routes to only when relevant. Doc count is not the enemy.
- **Zero expirable documentation.** Prices, specific vendor-as-THE-answer, version numbers — all must go.
- **Compress, don't delete.** Content that has value stays — distributed across focused routed docs rather than crammed into one mega-file.
- **Automation over discipline.** Where the framework relies on AI discipline alone (e.g., "do not summarize rules"), add a machine-verifiable gate.
- **Only Phase 1 tested so far.** Don't over-optimize bootstrap while Scaffold/Develop/Maintain are unexplored.

Surface tests since Step 32 surfaced the same pattern: framework is biased (JS in conventions prose, HIPAA in compliance examples, REST in API conventions) and missing routing (mobile decision tree, GraphQL, compliance regimes beyond HIPAA, audit-log vs app-log). None are "needs more content" — all are "needs categorical routing refactor."

Changes:

**1. Expirable content purged.**
- ONBOARD.md stripped of hardcoded prices ($100/mo compliance floor, $150/mo EKS baseline, $0 kind, $15 product example). Replaced with categorical language ("compliance infra has meaningful monthly floor; research current vendor quotes before committing").
- Convention #0 Wrong/Right compressed: named ONE canonical example per pattern with "current market leader — research at bootstrap" framing, instead of 4-7 specific vendors per example.

**2. ONBOARD.md split (compress, don't delete).**
- `bootstrap/ONBOARD.md` remains the linear flow (~400 lines, down from 696) — still covers steps 1-6 but routes out for depth.
- `bootstrap/RED-FLAGS.md` (new) — the 10-row Red Flag Combinations table plus detailed playbooks (vague-regulated default, mobile disambiguation, deploy-gate, priority-ranking fallback, budget-vague).
- `bootstrap/EXISTING-PROJECT.md` (new) — Parts A/B/C/D extracted (~250 lines). Rule extraction, cross-reference, doc migration.
- `bootstrap/LEARNING-PROJECTS.md` (new) — learning-intent detection, exception flow, cost-minimization patterns for learning-oriented projects. Generalized beyond Kubernetes to any enterprise-infra-as-learning-vehicle.

**3. Categorical routing added (not content, just pointers).**
- Mobile decision tree pattern in ONBOARD.md Group 2 — parallel to web's non-technical/developer/experienced ladder. Native-capability-required vs installable-only vs read-on-phone. Not a vendor list.
- Compliance-regime routing in Step 2 — categorical: HIPAA / SOC2 / PCI / GDPR / CCPA / sector-specific. Each a research-trigger, not a vendor answer.
- GraphQL routing pointer in convention #9 and backend/B2: "if server uses GraphQL, research schema design, query-complexity limits, field-auth, DataLoader patterns."
- Convention #15 lint rules refactored from 4 TS-specific examples to categorical patterns ("native linter for the language, rules catching untyped escape hatches, console-level output, unsafe casts, direct third-party UI imports").
- Convention #23 + B4: audit-log vs app-log distinction — different retention, mutability, access controls. Pattern named, not implementation.
- references-mobile.md: Backend section added for fullstack mobile.

**4. Machine-verifiable gate.**
- `scripts/validate-migration.sh` (new) — replaces "AI discipline" for existing-project migrations. Checks: every rule in original CLAUDE.md has a pointer in INDEX.md, every override file is non-trivial length (catches summarization), every audit file names its convention, no original files modified, migrated docs match originals byte-for-byte.

**5. Adversarial robustness fix.**
- Added rule for "no stack preference" vague answer (parallels vague-regulated). User saying "use whatever" is a deflection, not a choice — apply Step 3 default (platform-first if one covers 80%+, otherwise minimum-viable custom).

Deferred (Phase 2-4 pivot):
- Scaffold phase agent-testing
- Multi-tenant org-isolation routing in #24 (minor, can fold into #24 later)
- #11 provider list refactor to categorical (current list is OK pending evidence of drift)
- Runbook template for platform admin tasks

## 2026-04-17 (Step 32) — Round 3 verification fixes

Trigger: 4 Step 31 verification agents completed (blog-v3, therapy-v3, vague-v3, shopify-v3). All four confirmed Step 31 fixes fired correctly. Shopify-v3 also verified `templates/references-platform.md` is "categorically better" than v2 hand-written; both `claude-settings` variants tested against wrong layout to confirm silent-failure mode fixed. Real improvements:
- blog-v3 changed outcome from EKS ($150/mo) to kind local ($0) via learning-project exception
- therapy-v3 used new references-platform.md cleanly, same outcome as v2 but faster/cleaner artifacts
- vague-v3 correctly assumed regulated (PHI-safe default) and picked responsive+PWA manifest instead of silent React Native

Agents surfaced new, smaller gaps. Prioritized by silent-wrong > polish heuristic.

Fixes (8 items, all small edits):

1. **PWA vs responsive distinction clarified** — vague-v3 collapsed them into one choice. Framework now presents them as genuinely separate decisions in the decision table, with an explicit "cheapest default is plain responsive; PWA adds install-to-home-screen capability on top." Forces AI to pick one, not merge.

2. **Proactive learning-project heuristic** — blog-v3 needed three turns to surface the learning intent. If user opens with solo + enterprise-infra declaration (K8s, Redis, service mesh, Kafka, multi-region) for a personal/small project, framework now instructs AI to ask ship-vs-learn in the FIRST reply, not after red flags fire.

3. **Hard-block for vague-regulated at deploy gate** — vague-v3 correctly assumed regulated but the question stayed open through bootstrap. A user who keeps saying "defaults" could launch on Railway/Render without a BAA. Framework now adds a pre-deploy rule: "If regulated-data question is unresolved (default-assumed yes, not explicitly answered), scaffolding and deploy steps must halt until the user affirmatively answers." Documented in ONBOARD.md and flagged in VERSION-LOG template.

4. **Okta/SSO tenant ownership verification** — Added to Red Flag Combinations table: if user mentions enterprise SSO, verify they control the IdP before scaffolding auth. Prevents generating a WorkOS-Okta integration the user cannot actually provision.

5. **Priority-ranking fallback** — vague-v3 correctly forced the ranking conversation but the user refused. Agent improvised a ranking on its own. Framework now specifies the default fallback order when user refuses to rank: compliance > data integrity & auth > core user flow > scale > polish. AI documents the applied order in References.md.

6. **Budget-vague handling** — vague-v3 accepted "small I guess" as a free-form answer. Framework now adds a parallel to the vague-regulated rule: if budget is vague AND regulated-data is yes, surface the $100/mo minimum compliance floor before scaffolding; if regulated is no, assume $0 budget and surface cost of any non-free component before committing.

7. **Convention #0 learning-project carve-out in Wrong/Right** — blog-v3 agent noted that #0's blog example says "Astro or Ghost. Done." with no carve-out. A future AI reading only #0 (not ONBOARD.md) could still mis-recommend. Added a bullet: "Exception: learning projects — see ONBOARD.md Step 3 for the carve-out."

8. **references-platform.md polish**:
   - Added pricing-caveat line: "verify current pricing on vendor's site before signup; template pricing is a reference only."
   - Added git-init guidance: "Platform projects may still version-control References.md itself and any exported config/theme files. Use your judgment."
   - Restructured Configuration Checklist into generic-first-then-sector-specific: the five baseline sections (Account, Domain, Legal, Security, Documentation) apply to every platform; the sector-specific sections are labeled [CONTENT/BLOG], [COMMERCE], [BOOKING], [HEALTHCARE], [WORKFLOW] so AI picks the applicable block and skips the others.
   - De-duplicated Build Approach vs Applicable Conventions (shopify-v3 noted the repetition).

9. **ONBOARD.md checklist conditional** — Shopify-v3 caught that the post-bootstrap checklist still lists `docs/systems/` and `docs/features/` as required, but Step 4 platform branch says skip them. Added conditional: "For custom builds only" tag on those rows.

10. **templates/feature-tree-platform.md (NEW)** — Step 4 platform branch says "feature-tree.md reshaped as a configuration checklist" but there was no template. Shopify-v3 invented a milestone structure (Launch / Growth / Mature / Deferred). Shipping that structure as a template so future platform-path agents don't re-invent.

Deferred:
- Proactive "solo + enterprise infra = learning intent >80%" heuristic (captured in #2 above via different trigger)
- `#26 Deployment/Orchestration` convention (larger lift, marginal value)
- K8s scaffolding template (scaffold-phase concern, not bootstrap)

## 2026-04-17 (Step 31) — Round 2: autonomous-baseline fixes

Trigger: 4 more agent runs (2 fix-verification + 2 deferral tests) against Step 30 framework. Fix verification confirmed Step 30 works — therapy-v2 cleanly reached a platform without needing to override Step 2; blog-v2 pushed back 3 times (vs 0 in round 1). Deferral tests confirmed two HIGH gaps that cause silent wrong outcomes:
- Vague answers to "sensitive data" question default to "not regulated" — legally dangerous for HIPAA/PCI scenarios.
- Mobile "phone app" silently routes to React Native — PWA/responsive often cheaper and sufficient.
- `templates/claude-settings.json` paths are 404 for new-project clone installs — Claude Code silently skips missing hooks, zero error, zero warning. Confirmed by `ls` test.
- `templates/references-platform.md` doesn't exist; 80% of custom templates unusable for platform-choice outcomes.

Design heuristic captured in user philosophy memory: "if AI can do good autonomously, it can do great with guidance." Gaps that cause silent wrong outcomes without human intervention are higher priority than gaps that merely produce suboptimal outcomes human can steer through. This shaped Step 31 scope.

Fixes (7 items):

1. **`templates/references-platform.md` (new file, ~80 lines).** Structure proposed by the Shopify-persona agent: Project, Build Approach, Platform, Why {platform}, Decisions Log, Configuration Checklist, Applicable Conventions, Out-of-Scope Systems, Convention Overrides, Open Questions. For the "user picked Shopify/WordPress/Blueprint" outcome.

2. **`templates/claude-settings.json` → two named variants.** Split into `claude-settings.injected.json` (paths include `archetype/` segment, for after `inject.sh`) and `claude-settings.root.json` (no `archetype/` prefix, for new-project clone). Removed the ambiguous single file. `bootstrap/hooks/README.md` updated to point at the right one by install method.

3. **Vague sensitive-data default rule.** ONBOARD.md now instructs: if user answers Group 5 (sensitive data) with "I dunno" or equivalent vague answer, default to "assume regulated" and surface the assumption. Legal-risk direction is the safe default.

4. **Mobile disambiguation rule.** ONBOARD.md Group 2 now requires an explicit follow-up when user says "phone app": native (App Store), PWA (installable web), or responsive web. Framework no longer silently picks React Native.

5. **Convention #0 Blueprint → SimplePractice.** Blueprint is measurement-based-care-centric, not generic document-sharing; SimplePractice is the industry-standard HIPAA therapy platform. Wording polish in convention #0.

6. **Learning-project discovery question.** New Group 1 sub-question: "Is this a product to ship, or a project to learn a technology?" If learning, platform-vs-custom research is bypassed — custom IS the point. But scale-vs-cost still matters (learning K8s on a $80/mo EKS cluster is costly; kind/minikube locally is free).

7. **Step 4 platform branch.** Explicit "For platform choice" subsection at Step 4 start. Previously only a one-line redirect from Step 3.

Deferred again:
- Mid-discovery summary checkpoint (low-leverage relative to cost)
- Hard-stop for scale mismatch (learning-project question addresses the common case)
- VERSION-LOG Type field for platform choice (cosmetic)
- Conventions.md lookup table "Platform-only project" row (cosmetic)

## 2026-04-17 (Step 30) — Bootstrap audit fixes

Trigger: 5-agent audit of framework bootstrap (3 plan-only agents + 2 real execution agents on `/tmp/archetype-test-therapy/` and `/tmp/archetype-test-blog/`). All 5 converged on the same structural defects. Real-run blog agent generated EKS + Redis + Postgres + ArgoCD + Prometheus for a 5-reader personal blog because the framework gave it license to fold. Real-run therapy agent barely recovered a HIPAA-appropriate platform choice (Blueprint) because Step 3 research saved it — but Step 2's compliance branch would have routed a solo $0-budget therapist straight to AWS Cognito+RDS+IaC if a less-careful AI had followed it literally.

Six critical fixes to bootstrap/ONBOARD.md + conventions/00-reusability.md:

1. **Rewrote the "Developer who knows what they want" example** (ONBOARD.md:310-314). The shipped example modeled capitulation ("Perfect. Let me generate..." with zero research). The new version shows the AI running Step 3 briefly even for a confident user, confirming the use case isn't platform-served before proceeding. Both real-run and plan-only agents explicitly cited this example as the line that trained them to fold.

2. **Reordered Step 2 compliance branch vs Step 3.** Old: regulated data → AWS (regardless of scale/budget). New: regulated data → run Step 3 platform research first; custom AWS path only if platform doesn't exist AND user can absorb ~$100/mo minimum compliance infra cost.

3. **Qualified line 165 "AI respects preferences" clause.** Old: unqualified compliance. New: AI respects preferences AND still runs Step 3 research if platform covers the use case or scale/cost mismatch is severe.

4. **Added scope-change handler to Step 2.** Linear discovery had no loop-back. Now: if user adds a requirement mid-discovery (multi-user after "solo", SSO after "personal", mobile after "web"), framework tells AI to acknowledge, re-ask affected groups, re-run Step 3 research.

5. **Fixed Convention #0 miscitation and extended #0.** Old: ONBOARD line 186 cited #0 for scale-fit ("don't build what already exists") but #0 was about intra-project code reuse only. New: extended #0 to cover external reusability ("don't rebuild what already exists as a market-available platform"). Citation now honest.

6. **Added Red Flag Combinations table.** 8 known-bad combinations (free + regulated data, offline + PHI, solo user + enterprise infra, real-time + static hosting, multi-user + local-only stack, enterprise SSO + consumer auth, etc.). AI must surface these before moving to Step 3.

Design decision: all fixes are edits to existing files. No new convention, no new template in this session. The goal is to harden Step 3's defenses — Step 3 is the framework's strongest mechanism, and closing its flanks is higher leverage than adding new mechanisms.

## 2026-04-17 (Step 29 follow-up) — update.sh self-replace bug

Trigger: First deployment of Step 29 to game-test crashed mid-run with "syntax error near unexpected token `fi`". Root cause: update.sh includes itself in the UNIVERSAL_FILES copy loop, so it replaces itself while still executing. `cp` truncates-and-writes the destination, corrupting the running bash's open read descriptor. Re-running succeeded (second run used the already-copied new version), but the failure mode is unacceptable.

Fix: atomic self-replace.
- update.sh removed from UNIVERSAL_FILES (main copy loop no longer touches it)
- Added a dedicated Step 9 at the very end of update.sh that does: `cp new update.sh.new` → `mv update.sh.new update.sh`. The mv is a rename, which unlinks the old inode but keeps it alive as long as bash holds it open. The running script continues reading the old content from its fd while future executions get the new file.
- Comparison output still lists update.sh as "CHANGED (self-replace at end)" so users see it

Files: dist/update.sh only. Convention count unchanged.

Lesson for future framework scripts: never let a running script overwrite itself with plain cp. Use cp-then-mv or write to a `.new` and handle on next run.

## 2026-04-17 (Step 29)

Trigger: Plan audit identified framework at 8/10 design, 6/10 battle-testing. Six-improvement hardening plan proposed. Critical review cut four items (dependency graph, 5 of 7 hooks, ESLint-specific files, synthetic test specs) as low-value relative to cost.

Research: Confirmed hooks are actively used in 2026. Claude Code has 21 lifecycle events, PreToolUse blocks actions, event JSON on stdin, exit code 2 blocks with context.

Changes:
- scripts/validate-framework.sh (new): self-test script. Checks file path references in CLAUDE.md, convention count consistency, #N references resolve, required sections in convention docs, no duplicate numbers, backend paths. Auto-detects framework location.
- conventions/25-automated-enforcement.md (new): convention #25. Lint rules catch violations at write time. Framework-agnostic (research notes cover ESLint, Ruff, golangci-lint, Roslyn).
- bootstrap/hooks/pre-destructive-warn.sh (new): PreToolUse hook. Blocks destructive commands (rm -rf, git reset --hard, git push --force, DROP TABLE, TRUNCATE, chmod -R 777, dd, mkfs). Reads JSON from stdin, exits 2 with stderr message.
- bootstrap/hooks/post-task-verify.sh (new): Stop hook. Advisory verification checklist. Non-blocking.
- bootstrap/hooks/README.md (new): install guide for Claude Code, Cursor, other tools.
- templates/claude-settings.json (new): Claude Code hook configuration template.
- templates/session-review.md (new): 5-question periodic review template.
- templates/hooks-spec.md: updated to point to real scripts in bootstrap/hooks/.
- development/MAINTAIN.md: added Session Reviews section.
- Conventions.md: added #25 entry in Quality category, updated count 25 → 26.
- backend/Conventions.md: updated universal count 25 → 26.
- README.md: updated count and added #25 mention.
- update.sh: added scripts/ to UNIVERSAL_DIRS.
- inject.sh: added scripts to copy list.

Cut from original plan:
- Convention dependency graph (adding `## Dependencies` to 32 files with no clear consumer)
- 5 of 7 hooks (doc-freshness, feature-tree-audit, post-edit-doc-check, etc. — noise generators)
- ESLint-specific lint rule .js files (violates language-agnostic design)
- Synthetic test project specs (user battle-testing via real webpage beats simulation)

Design decision: trimmed plan from 21-29 hours to ~6-9 hours. Delivers 80% of value at 25% of cost. Framework does not need a dependency graph, 7 hooks, or synthetic test specs.

Convention count: 25 universal + 7 backend = 32 total → 26 universal + 7 backend = 33 total.

## 2026-04-16 (Step 28)

Trigger: Bootstrap jumps to creating projects too quickly. It picks a tech stack from its training data without researching whether that's the best fit. A bakery website might be better as WordPress than React. An e-commerce site might need Shopify not a custom build. The AI should research current options and present trade-offs BEFORE committing to a stack.

Changes:
- ONBOARD.md: added research phase between discovery and generation
- The AI must research online (if capable) or present known options with trade-offs BEFORE picking a tech stack
- Research considers: build vs buy (WordPress, Shopify, Squarespace vs custom), framework fit (Astro for content, Next.js for interactive, etc.), hosting fit, and whether the user actually needs a custom app
- The AI presents options with pros/cons and waits for user decision
- Only after the user confirms does the AI proceed to generate References.md

Design decision: the framework should prevent over-engineering. Not every idea needs a custom React + Node app. Sometimes WordPress + a theme is the right answer. The bootstrap should be honest about this.

## 2026-04-14 (Step 27)

Trigger: Review found #11 still has authorization and security content after the split. Three conventions (#11, #23, #24) cover overlapping topics. AI gets confused about which is authoritative. Also "23 conventions" count stale in multiple files.

Changes:
- Convention #11: pruned to pure authentication only. Removed authorization content (moved to #24), removed security content (moved to #23). #11 now covers ONLY: identity, tokens, auth providers, auth utility wrapping.
- Updated convention count from "23" to "25" in CLAUDE.md, Conventions.md, README.md, ONBOARD.md, ARCHITECTURE.md, PLAN.md, BACKEND-AUDIT.md.
- Fixed backend/Conventions.md: "23 conventions for any project" → "25". Updated #11 references from "auth/security" to "authentication", #11 security reference to #23.
- Fixed README.md enforcement rule count: 16 → 21.
- Committed all pending work cleanly.

## 2026-04-13 (Step 26 - IMPLEMENTED)

Trigger: External plan review identified security as significantly under-covered. Convention #11 is 90% auth, 10% security. AI generates 2.74x more security vulnerabilities but the framework has no dedicated security convention.

Changes:
- Convention #11: renamed to Authentication. Added auth provider guidance (Supabase, Clerk, Auth0, Cognito, Firebase, ASP.NET Identity, Django auth). Authorization and security content split to #23 and #24.
- Convention #23: NEW - Application Security (OWASP, input validation allowlisting, CORS/CSRF, security headers, encryption at rest/transit, session management, error leakage, PII/data retention, supply chain security, audit logging, rate limiting, secret management)
- Convention #24: NEW - Authorization (RBAC/ABAC, centralized permission service, service-layer enforcement, object-level access checks, least privilege, authorization denial logging)
- Conventions.md: added Security category, 3 new lookup table rows (permissions, input validation, security review)
- CLAUDE.md: added 2 enforcement rules (21 total): input validation at entry points, authorization at service layer

Design decision: security conventions go in universal (not backend-specific) because frontend also needs input validation, XSS prevention, CORS handling, and secure storage. Total conventions: 25 universal + 7 backend = 32.

## 2026-04-13 (Step 25 - IMPLEMENTED)

Trigger: Framework covers frontend well but backend has 7 categories of gaps.

Research: 37 backend-specific conventions across 7 categories. Audited against universal: 4 overlaps referenced not duplicated, 4 redundancies excluded, 30+ genuinely new.

Implemented:
- backend/CLAUDE.md: 11 backend enforcement rules supplementing the 19 universal rules
- backend/Conventions.md: lookup index with task-to-convention mapping
- B1-database: N+1 prevention, migration safety, transactions, connection pooling, indexes, soft delete, ORM patterns
- B2-api-design: URL design, HTTP methods, status codes, pagination, validation, response envelope, versioning
- B3-middleware: ordering, request lifecycle, cross-cutting concerns, correlation IDs
- B4-logging: structured logging (never console.log), levels, sensitive data redaction, health checks, metrics
- B5-background-jobs: async vs sync, idempotency, retry with backoff + DLQ, job design
- B6-file-handling: presigned URLs server-side, magic byte validation, storage abstraction
- B7-caching: when to cache, TTL invalidation, cache layers, stampede prevention
- Root CLAUDE.md: routes to backend/CLAUDE.md for backend projects

## 2026-04-13 (Step 24)

Trigger: Second external review of CLAUDE.md enforcer. 8 suggestions audited, 2 already implemented (confirm before coding, mid-task re-scan from Step 20), 6 genuinely new.

Changes:
- "If uncertain, ask" moved from last rule to first rule. Most important safety valve was buried at bottom.
- Added meta-rule: "Never modify CLAUDE.md, Conventions.md, or convention docs without explicit permission." Prevents AI from weakening its own rules.
- Added codebase search rule: "Search the codebase for existing implementations before building. feature-tree.md may be stale — the code is the source of truth."
- Added commit discipline: "Commit after every verified change. Each commit is a rollback point." Convention #2 had this but the enforcer didn't.
- Expanded convention #0 inline summary from one-liner to actionable decision points (check what exists → use it or build reusable).
- Added database/migration row to Conventions.md lookup table.
- CLAUDE.md enforcement rules: 16 → 19

## 2026-04-13 (Steps 20-23)

### Step 23: Bootstrap and scaffold logging to VERSION-LOG.md
Trigger: VERSION-LOG.md existed but only update.sh wrote to it. Bootstrap and scaffold left no trace of what they did.

Changes:
- ONBOARD.md Step 5: after bootstrap, AI logs to VERSION-LOG.md (date, type, tech stack, files generated with counts, conventions read, discovery answers, key decisions)
- SCAFFOLD.md: after scaffold, AI appends to VERSION-LOG.md (date, sessions, systems built with locations and convention numbers, systems skipped, deps installed, verification status)
- A future AI or developer can now trace exactly what happened during each phase

### Step 22: update.sh and VERSION-LOG.md
Trigger: Need non-destructive framework updates across multiple projects. Manual syncing doesn't scale.

Changes:
- Created update.sh: pulls latest from GitHub, compares, shows diff, asks confirmation, applies
- Universal files overwritten (conventions, templates, phase docs, CLAUDE.md). Project-specific NEVER touched (References.md, feature-tree.md, overrides, protocols, catalogs, docs, todo)
- conventions/overrides/ preserved when updating conventions/ (backup → overwrite → restore)
- VERSION-LOG.md: created by inject.sh (bootstrap commit hash) and appended by update.sh (date, commit hash per update)
- inject.sh updated: now also copies update.sh and creates VERSION-LOG.md
- Tested on game-test project: update detected changed files, applied cleanly, logged commit e5f2db1

### Step 21: archetype/ as permanent engine (not deprecated)
Trigger: Discussion about whether archetype/ should be removed after bootstrap

Decision: archetype/ stays permanently. It's the engine, not disposable scaffolding. Reasons:
- Convention docs are read during development, not just bootstrap
- Templates used when creating new features
- DEVELOP.md and MAINTAIN.md are ongoing reference
- update.sh needs the engine to compare versions
- Project runs FROM root (CLAUDE.md, References.md). archetype/ powers from behind.

### Step 20: CLAUDE.md root placement, verification, bootstrap gate, scope expansion, hooks guidance
Trigger: External review identified 5 issues with enforcer effectiveness

Changes:
- inject.sh: copies CLAUDE.md to project root (Claude Code auto-reads), archives existing
- CLAUDE.md: added verification checkpoint ("state which conventions you read before first code output")
- CLAUDE.md: added bootstrap gate ("if References.md doesn't exist, run bootstrap first")
- CLAUDE.md: added scope expansion ("re-scan Conventions.md if task scope changes mid-work")
- hooks-spec.md: added setup instructions for Claude Code, Cursor, other tools. Made clear hooks optional but AI can help wire them up.

## 2026-04-12 (Step 19)

Trigger: External review identified 6 issues with enforcer effectiveness

Changes:
- CLAUDE.md: lazy loading ("scan Conventions.md, read ONLY relevant docs, not all 23")
- CLAUDE.md: workflow gate ("Before Writing Code" section forces convention lookup before coding)
- CLAUDE.md: cross-session memory warning ("do not assume you remember from previous session")
- Conventions.md: reframed as lookup index with explicit "This is a LOOKUP INDEX" header
- Conventions.md: added task-to-convention mapping table (10 task types → which conventions to read)
- Issue #6 (inline key rules) solved by workflow gate, no separate fix needed

## 2026-04-11 (Step 18)

Trigger: Doc audit on MobileMed frontend found stale, misfiled, and misaligned docs

Changes:
- Added Part D to bootstrap: documentation discovery, migration, and audit
- Discovery searches entire project for docs (not just /docs/), categorizes into 5 tiers
- AI asks user when ambiguous instead of guessing
- Migrated docs audited against conventions (alignment, violations, staleness, conflicts)
- Fixed 15 doc issues in MobileMed frontend: stale Redux wording, misfiled TYPOGRAPHY-SYSTEM, deprecated design-system-integration, added factory/widget pattern to 5 docs
- Created archetype/todo/ folder with 4 actionable code audit findings

## 2026-04-10 (Step 17)

Trigger: Extracted files from MobileMed CLAUDE.md but nothing referenced them

Changes:
- CLAUDE.md: lists conventions/overrides/, protocols/, catalogs/, INDEX.md as additional read locations
- References.md template: added "Project-Specific Documentation" section for extracted files
- Convention doc template: added "Project Overrides" section referencing overrides/{N}-{name}.md
- ONBOARD.md: Part C added - cross-reference everything, create INDEX.md, verify every section mapped

## 2026-04-10 (Step 16)

Trigger: Bootstrap on MobileMed frontend lost 32 items from 1047-line CLAUDE.md (7 critical protocols)

Changes:
- Added Part B "Rule Extraction" to existing-project bootstrap
- New extraction structure: conventions/overrides/, protocols/, catalogs/, CLAUDE.md.additions
- Critical rule: "do not summarize. Extract in full. Preserve original wording."
- Re-ran on MobileMed frontend: 30 files extracted, line-by-line verification passed

## 2026-04-03 (Step 15)

Trigger: Need safe migration path for existing projects

Changes:
- Created inject.sh script for injecting framework as subfolder
- Archives existing CLAUDE.md before overwriting
- Creates FRAMEWORK-SOURCE.md with pointer back to source repo

## 2026-04-02 (Step 14)

Trigger: Scaffolding test on todo app produced raw Tailwind buttons instead of using MUI, skipped dark mode

Changes:
- Convention #6 (Styling): dark mode is ALWAYS required, not optional
- Convention #22 (Design System): must use established UI library (MUI, Chakra, Radix), not build from scratch
- CLAUDE.md: added "Never build standard UI components from scratch"
- SCAFFOLD.md: added theme light+dark and shared-components-use-each-other to checklist

## 2026-04-02 (Step 13)

Trigger: Full end-to-end test (bootstrap + scaffold + develop) revealed documentation gaps

Changes:
- SCAFFOLD.md: system docs must show exact import paths with project alias (@/)
- SCAFFOLD.md: .env.example required
- SCAFFOLD.md: "How to Add a New Feature" section required in References.md
- SCAFFOLD.md: database schema overview required (backend)
- SCAFFOLD.md: API endpoint summary required (backend)
- SCAFFOLD.md: shared UI components must use other shared components internally

## 2026-04-01 (Step 12)

Trigger: Simulation testing found 10 gaps in bootstrap flow

Changes:
- ONBOARD.md: bootstrap prompt tells agent to use Research Notes for fresh research
- References templates: added missing sections to match all scaffold steps (frontend: 10 added, backend: 5 added)
- Created references-mobile.md template
- Feature-tree template: added all 12+ system rows
- SCAFFOLD.md: added Database step for backend, File Storage step
- ONBOARD.md: existing project scan guidance (how to scan)
- ONBOARD.md: chains to SCAFFOLD.md as "Next Step"
- MAINTAIN.md: fixed grep -oP for macOS compatibility

## 2026-04-01 (Step 11)

Trigger: Vibe coder test showed bootstrap jumped to tech stack without understanding the user

Changes:
- Redesigned bootstrap entry: AI-guided discovery with 5 question groups
- Infrastructure depth detection based on user's experience level
- Non-technical → managed services, Developer → cloud platforms, Experienced → AWS, Enterprise → compliance-grade
- Example conversations for vibe coder and experienced developer

## 2026-03-31 (Steps 8-10)

Trigger: Convention docs had framework-specific code (React/MUI/RTK Query examples)

Changes:
- Rewrote all 23 conventions: descriptive concepts, no framework-specific code
- Convention docs describe WHAT to build and guide WHAT to research. Bootstrapper figures out HOW.
- Added Right vs Wrong examples to all conventions (research-backed from OX Security, GitClear, CodeRabbit)
- Removed JS/Node bias from scaffold, templates, and conventions (multi-language examples)
- Filled 5 frontend gaps from QA against original CLAUDE.md files (breaking change protocol, file upload, offline support, tech debt tracking, doc formatting)

## 2026-03-31 (Steps 6-7)

Trigger: Audit against initial plan found deviations

Changes:
- CLAUDE.md: made rules universal (not frontend-biased)
- Added Phase 3 (DEVELOP.md) and Phase 4 (MAINTAIN.md)
- Added feature-tree rule and documentation rule to CLAUDE.md
- Created global-claude.md template for behavioral rules
- Created feature-doc-template.md
- Architecture doc rewritten to reflect 4-phase model, feature tree, hooks

## 2026-03-30 (Steps 4-5)

Trigger: User identified that three things were mixed: behavioral rules, engineering DNA, technical specifics

Changes:
- Separated into layers: CLAUDE.md (enforcer) → Conventions.md (index) → conventions/ (docs) → References.md (project)
- Conventions made framework-agnostic
- References.md is the only per-project file

## 2026-03-29 (Steps 1-3)

Trigger: Review of 5 existing CLAUDE.md files + industry research

Changes:
- Created master rule sets (backend + frontend)
- Audited against Claude Code system prompt - removed redundant rules
- Research: instruction budget (~150 slots), human-steered > autonomous, verification highest leverage, security can't be default
- Initial framework concept: lean enforcer + convention docs + per-project references

## Origin

Started 2026-03-29 from a request to review 5 CLAUDE.md files:
- unira (React Native/Expo) - 130 lines
- unira-dashboard (React dashboard) - 135 lines + 8 hooks
- gmeblockchain-api (Node.js serverless) - 200 lines
- Development3/backend (TypeScript/SST) - 415 lines, 23 rules
- Development3/frontend-dashboard (React/MUI) - 600+ lines (later found to be 1047)

Goal: create a universal AI development framework that works across any language, framework, or AI assistant.
