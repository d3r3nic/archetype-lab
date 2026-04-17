# Framework Changelog

Every improvement to the Archetype framework, why it was made, and what triggered it.

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
