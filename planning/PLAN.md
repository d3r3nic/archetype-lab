# Framework Development Plan

## Origin

Started 2026-03-29 with a request to review 5 existing CLAUDE.md files across two project ecosystems and cross-reference with industry research on what actually works in AI-assisted development.

Projects analyzed:
- unira (React Native/Expo mobile app) - ~130 line CLAUDE.md
- unira-dashboard (React dashboard) - ~135 line CLAUDE.md + 8 hook scripts
- gmeblockchain-api (Node.js serverless) - ~200 line CLAUDE.md
- Development3/backend (TypeScript/SST) - ~415 line CLAUDE.md, 23 rules
- Development3/frontend-dashboard (React/MUI) - ~600+ line CLAUDE.md

## Key Research Findings

Industry research (2025-2026 proven practices, not hype):

1. Instruction Budget: Claude Code has ~150-200 instruction slots. System prompt uses ~50. Every redundant rule makes ALL rules less likely to be followed. Boris Cherny (Claude Code creator) keeps his to ~100 lines.

2. Human-Steered > Autonomous: Amazon had 4 Sev-1 production incidents in 90 days from unsupervised AI coding. METR study found experienced devs 19% slower with unsupervised AI. Martin Fowler's team recommends "human-on-the-loop" model.

3. Verification is Highest Leverage: Running tests after every change can 2-3x output quality.

4. Security Cannot Be Left to Defaults: AI-generated code has 2.74x higher vulnerability rates. Every AI-built app in one study lacked CSRF protection.

5. DORA 2025: AI amplifies existing practices. Good rules + steering = better results. Bad practices just get worse faster.

6. System Prompt Already Covers: clean code, no over-engineering, delete unused code, don't add features beyond asked, OWASP awareness, git safety. These do NOT need to be in CLAUDE.md.

## Evolution of the Idea

### Step 1: Master Rule Sets

First attempt was creating two master rule sets (backend + frontend). 18 rules each. Reusable templates with a project-specific section to fill in.

Problem identified: these still mixed behavioral rules, engineering philosophy, and project-specific technical details in one file.

### Step 2: Audit Against System Prompt

Audited every rule against Claude Code's actual system prompt (v2.1.87). Found significant redundancy. Rules like "remove unused imports" and "don't over-engineer" are literally in the system prompt already. Having them in CLAUDE.md wastes instruction budget.

Also found missing rules: plan before complex work, stop and ask when uncertain, folder structure awareness, dependency awareness.

### Step 3: The Lean Enforcer Insight

Key realization from the user: CLAUDE.md must stay lean. Too large and it misses rules. It should be an enforcer with zero context - catching when AI is about to go wrong and redirecting it to the correct place. A bloated CLAUDE.md means the AI never gets the chance to do what we designed it to do.

This is backed by research: the leaner the instruction file, the higher the compliance rate.

### Step 4: Separation of Concerns

The user identified that three things were being mixed:

1. Behavioral rules (how AI collaborates with the user) - universal, never changes
2. Engineering DNA (reusability-first, config-driven, etc.) - universal philosophy, framework-agnostic
3. Technical specifics (handler patterns, MUI wrappers, Redux conventions) - project-specific

These should be separate documents at separate layers.

### Step 5: The Reference Injection Pattern

Key design: References.md is the same filename in every project but has different content. It contains project scope, tech stack, and how each convention applies in this specific framework. This makes the rule system injectable - the rules don't change, only the project context does.

### Step 6: Nested Convention Layers

Reusability is just one convention. There are many: theming, single source of truth, config-driven design, boundaries, state architecture, etc.

Each convention needs layers of its own:
- Convention index (one-liner in Conventions.md)
- Convention doc (principle + reasoning + violations - framework-agnostic)
- Implementation details (in References.md - framework-specific)
- Deep docs (generated per project as it evolves)

The convention docs are like textbook chapters. They rarely change. They explain WHAT and WHY, not HOW. The HOW is always framework-specific and lives in References.md.

### Step 7: AI-Era Convention Shifts

The user identified that coding conventions have evolved with AI. Some conventions existed for human readability but now code is primarily written by AI and verified by humans. This changes what we optimize for:

- Folder structure is MORE important (it's how AI scopes context)
- Comments shift from "explain to humans" to "explain intent and constraints"
- Documentation shifts from tutorials to machine-parseable specifications
- Code organization optimizes for context window management, not human browsing
- File size should be smaller (AI context management)

But the core principles don't change - reusability, DRY, single source of truth. The REASONS change. You DRY code not because a human has to maintain copies, but because AI gets confused by multiple sources and doesn't know which is canonical.

New conventions that didn't exist before AI:
- Verification loops (run tests after every change)
- Plan-before-implement (AI drifts without plans)
- Incremental changes (AI can't self-correct across large batches)
- Context management (keeping things scopeable for AI)

### Step 8: The Self-Bootstrapping Framework

When a new project starts:
1. AI reads the lean enforcer (CLAUDE.md)
2. AI reads the convention catalog (Conventions.md)
3. User says "new [framework] project for [purpose]"
4. AI generates References.md by applying each convention to the chosen framework
5. AI generates folder structure following the conventions
6. Project is born with the right DNA already embedded

The framework doesn't just enforce rules - it creates the right project foundation.

### Step 9: Framework That Develops the Framework

The framework project itself needs to be developed using AI. So the framework needs to guide its own development. This is the meta-level: the planning folder is the source code, the dist folder is the build output. The framework is a product being built, and the product is a tool for building products.

### Step 10: The Meta-Convention

The user identified that reusability isn't convention #6 sitting alongside 32 others. It's convention #0 - the meta-convention that governs HOW every other convention gets implemented.

When any convention is applied to a project, it produces a reusable foundational system. Error handling becomes ONE error system. Theming becomes ONE theme system. The API layer becomes ONE API layer. Every feature plugs into these systems rather than building ad-hoc.

This means the bootstrap protocol isn't a separate thing - it's what happens when you apply #0 to each convention at project creation time. The framework doesn't just tell AI "error handling should be good." It says "build ONE reusable error handling system, and here's what it produces."

### Step 11: Final Catalog Assembly

Applied merges to reduce 33 conventions to 25 (1 meta + 22 core + 2 optional):
- Animation merged into Styling
- Async merged into Error Handling
- Code Quality merged into Build/CI
- AI Collaboration merged into AI Steering
- AI-Optimized Code woven into Project Setup, Type Safety, Documentation
- Dependencies + Environment merged into Project Setup
- i18n moved to optional addon
- Observability moved to optional addon

Cross-referenced every convention against all 4 sources. Duplicates resolved. Best source identified per convention.

### Step 12: Audit and Deviation Fixes

Audited the built product against the plan. Found deviations:
- CLAUDE.md evolved from "behavioral rules" to "direct enforcement rules with redirects" (intentional - user requested)
- Convention docs had framework-specific examples without labels (fixed: labeled as illustrative, added multi-framework examples)
- Architecture doc was stale (fixed: full rewrite reflecting 4-phase model, feature tree, hooks, routing table concept)
- References.md was described as "only per-project file" but feature-tree.md and docs/ are also per-project (fixed: architecture now lists all per-project artifacts as Layer 4)
- Behavioral rules had no home (fixed: created global-claude.md template for ~/.claude/CLAUDE.md)
- Phase 3 (Develop) and Phase 4 (Maintain) had no documents (fixed: DEVELOP.md and MAINTAIN.md created)
- Plan had duplicate sections (fixed: cleaned up)
- Missing templates: feature doc template (fixed: created feature-doc-template.md)

## Key Decisions (Final)

1. CLAUDE.md is a lean routing table. ~13 direct enforcement rules with redirects. Universal across frameworks.
2. Conventions are framework-agnostic. Examples may use specific frameworks but are labeled as illustrative.
3. References.md is the primary per-project file. feature-tree.md and docs/ are also per-project (Layer 4).
4. Convention docs account for AI-era shifts in why conventions matter.
5. The framework bootstraps new projects AND onboards existing ones.
6. Convention examples show multiple frameworks where helpful for clarity.
7. Documentation flows alongside code at every phase, not as an afterthought.
8. Reusability is the meta-convention (#0) that governs all others.
9. Every convention produces a reusable foundational system at project creation.
10. Bootstrap = applying #0 to each convention for the chosen framework.
11. CLAUDE.md has direct enforcement rules. Behavioral rules go in global ~/.claude/CLAUDE.md.
12. Feature tree is a living map maintained by hooks, audited periodically.
13. The product (dist/) is a separate repo from the factory (planning/, research/).

## Resolved Items

Previously undecided, now resolved:
- Convention doc template: DECIDED (convention-template.md created)
- CLAUDE.md enforcer format: DECIDED (direct rules with convention redirects)
- References.md template: DECIDED (frontend + backend templates created)
- Backend/frontend variants: DECIDED (conventions universal, References.md is framework-specific)
- System prompt conflicts: DECIDED (#0 explicitly notes it overrides "don't design for hypothetical futures")
- Convention dependencies: PARTIALLY (scaffold order in SCAFFOLD.md implies dependencies)

### Step 13: Real-world testing and the discovery flow

After Step 12 (audit fixes), tested the framework end-to-end with a vibe coder simulation. Found that the bootstrap jumped straight to picking a tech stack, which doesn't work for non-developers who don't know what to choose.

Added a discovery interview phase (5 question groups) that asks plain-English questions a non-developer can answer:
- What is it? (describe to a friend)
- Where does it run? (browser, phone, desktop)
- What do users do? (login, forms, uploads, real-time, offline)
- Scale and stage (personal, MVP, enterprise)
- Infrastructure and sensitivity (managed vs self-managed, sensitive data)

The AI reads the user's experience level from how they talk and adjusts:
- Non-technical -> Supabase + Vercel (managed services, zero DevOps)
- Developer -> managed cloud platforms with simple deployment
- Experienced -> AWS preferred, full control
- Enterprise/compliance -> AWS with encryption, audit trails

Tested with bakery owner ("I want something for my small bakery") and HR tool ("Windows office, Microsoft stuff, sensitive employee data"). Both scenarios produced correct tech stack decisions (Supabase + Vercel for bakery, ASP.NET Core + Blazor + Entra ID for HR tool).

### Step 14: Production-grade theme + established UI library

Real-world test on todo app showed scaffolding agent built buttons from scratch with raw Tailwind classes and skipped dark mode. Updated convention #6 (Styling) and #22 (Design System) to require:
- Dark mode is ALWAYS required, not optional
- Use established UI library (MUI, Chakra, Radix, etc.) - do not reinvent wheels
- Configure library theme with light AND dark from day one
- Wrap library components, features import from wrappers

Added CLAUDE.md rule: "Never build standard UI components from scratch. Use the established UI library, configured and wrapped."

### Step 15: Inject script for existing projects

Added inject.sh to the framework root. Copies framework files into a target project as a subfolder (default: archetype/) without modifying any existing files. Designed for migrating existing projects.

```
./inject.sh /path/to/existing-project
./inject.sh /path/to/existing-project custom-subfolder-name
```

This is the safe migration path. The framework lands in a sibling folder, then the existing-project bootstrap runs from there.

### Step 16: Rule extraction phase for existing projects

Tested the existing-project bootstrap on Development3/frontend-dashboard (1047-line CLAUDE.md). The bootstrap captured tech stack and feature tree but LOST 32 items including 7 critical workflow protocols (Feature Audit Protocol, Breaking Change Protocol, Technical Debt Tracking, Critical Workflow, Factory Decision Tree, Widget Pattern, Non-Breaking Fixes catalog).

Added Part B "Rule Extraction" to the existing-project bootstrap. Required outputs:
- conventions/overrides/{N}-{name}.md - per-convention project rules (full text, not summaries)
- protocols/{name}.md - workflow protocols
- catalogs/{name}.md - reference materials
- CLAUDE.md.additions - extra enforcement rules
- INDEX.md - master map

CRITICAL rule: do not summarize. Extract in full. If the original has 200 lines on a protocol, the extracted file has 200 lines.

Re-ran on frontend project. 30 files extracted. Verified line-by-line walkthrough of original Claude.md - every section now has a destination.

### Step 17: Cross-referencing extracted files

After extraction, the new files existed but nothing referenced them. A new AI reading the framework wouldn't find them.

Added Part C "Cross-reference everything":
- CLAUDE.md (the enforcer) lists conventions/overrides/, protocols/, catalogs/, INDEX.md
- References.md template has "Project-Specific Documentation" section listing extracted files
- Convention doc template references conventions/overrides/{N}-{name}.md if it exists
- INDEX.md is the master map

A new AI reaches any extracted file in 2 hops max: CLAUDE.md -> References.md/INDEX.md -> target file.

### Step 18: Documentation discovery + migration + audit (Part D)

The frontend project had a substantial /docs/ folder (12 numbered topic folders + standalone files) that wasn't part of the rule extraction. Added Part D to the existing-project bootstrap:

1. Discover all documentation in the project (not just /docs/) - search 8+ locations including README files in subfolders, /docs1/, /architecture/, /adr/, etc.
2. Categorize into 5 tiers (architectural, feature, history, stale, generated/temp)
3. Ask the user when ambiguous - don't guess
4. COPY (not move, not edit) into archetype/docs/migrated/ preserving structure
5. Map each migrated doc to a framework convention
6. Audit each migrated doc against its convention (alignment, violations, staleness, conflicts)
7. Create archetype/docs/audit/{path}.audit.md with findings
8. Create archetype/docs/audit/SUMMARY.md with status table

Critical rule: original /docs/ stays untouched. Copies and audits live entirely in archetype/.

### Step 19: CLAUDE.md lazy loading, workflow gate, session memory

External review identified 6 issues with the CLAUDE.md enforcer:

1. "Read everything upfront" defeats its own purpose. The AI either reads all 23 convention docs (context flooding) or reads only the index and skips the actual rules. Fixed: lazy loading instruction - "scan Conventions.md, identify which 2-4 conventions apply, read ONLY those."

2. No workflow gate before code generation. The 16 enforcement rules are reactive guardrails but nothing forces the AI to actually read the convention doc before coding. Fixed: added "Before Writing Code" section requiring the AI to identify applicable conventions and read them. Creates a visible checkpoint.

3. No cross-session memory protection. Every new session starts fresh but the CLAUDE.md didn't acknowledge this. Fixed: "Do not assume you remember conventions from a previous session."

4. Conventions.md reads like a table of contents, not a lookup index. Fixed: reframed header as "This is a LOOKUP INDEX. Do not read all 23."

5. No mapping from task types to conventions. Fixed: added quick-reference table mapping 10 common task types to which conventions to read.

6. Enforcement rules link to conventions but don't state the key rule inline. Not needed if #2 (workflow gate) is in place - the gate forces reading the full convention doc.

Total cost: ~12 lines across 2 files. CLAUDE.md: 38 lines. Conventions.md: 66 lines.

## Key Decisions Added

11. The bootstrap interviews the user in plain English instead of asking for tech stack upfront.
12. The AI reads the user's experience level from the conversation and adjusts infrastructure choices.
13. Dark mode and an established UI library are non-negotiable in the styling/design conventions.
14. Existing project bootstrap has 4 parts: scan codebase (A), extract rules (B), cross-reference (C), migrate and audit docs (D).
15. Rule extraction must preserve the full original text, not summarize.
16. Documentation discovery searches the entire project, not just /docs/.
17. When the discovery is ambiguous, the AI asks the user instead of guessing.
18. All extraction/migration is non-destructive: original files are never modified, only copied.

### Step 20: CLAUDE.md root placement, verification checkpoint, bootstrap gate, scope expansion, hooks guidance

External review identified 5 more issues:

1. CLAUDE.md only works if at project root (Claude Code auto-reads from root). Fixed: inject.sh now copies CLAUDE.md to project root, archives any existing one. Everything else stays in archetype/ subfolder.

2. No way to verify AI actually read conventions. Fixed: "Before your first code output, state which conventions you read."

3. AI fails at step 1 if bootstrap hasn't run (References.md doesn't exist). Fixed: bootstrap gate - "if References.md doesn't exist, run bootstrap first."

4. No rule for mid-task scope expansion. Fixed: "if task scope expands, re-scan Conventions.md."

5. Hooks template sits unused. Fixed: added setup instructions for Claude Code, Cursor, and other tools. Made it clear hooks are optional but the AI can help wire them up if asked.

### Step 21: Archetype as engine, not disposable scaffolding

Key architectural decision: the archetype/ folder is NOT deprecated after bootstrap. It stays as the engine.

The project architecture after bootstrap + promotion:
```
project/
├── CLAUDE.md              ← at root (auto-loaded by Claude Code)
├── References.md           ← at root (project context)
├── feature-tree.md         ← at root (project map)
├── conventions/            ← promoted from archetype/ (active use during development)
├── protocols/              ← promoted (project-specific workflows)
├── catalogs/               ← promoted (reference material)
│
├── archetype/              ← THE ENGINE (stays permanently)
│   ├── conventions/        ← source of truth for universal conventions
│   ├── bootstrap/          ← ran once, stays for reference
│   ├── scaffolding/        ← ran once, stays for reference
│   ├── development/        ← DEVELOP.md, MAINTAIN.md (ongoing reference)
│   ├── templates/          ← for generating new features/systems
│   ├── FRAMEWORK-SOURCE.md ← pointer back to repo for updates
│   └── (future: update.sh) ← pulls latest, applies non-destructive updates
│
├── docs/
└── src/
```

The flow:
- Bootstrap: archetype/ generates References.md, feature-tree.md, conventions/ at project root
- Development: AI reads root CLAUDE.md → root References.md → root conventions/. archetype/ is background engine.
- Updates: pull new archetype from repo → future update.sh compares versions → overwrites universal files (conventions/) → skips project-specific files (References.md, overrides, protocols) → flags new conventions to adopt

Why NOT deprecate archetype/:
- Convention docs are read during development, not just bootstrap
- Templates are used when creating new features
- DEVELOP.md and MAINTAIN.md are ongoing reference
- Update mechanism needs the engine to compare versions
- The engine is like a package dependency: it powers things but the project doesn't run FROM it

### Step 22: Non-destructive update mechanism (update.sh)

Created update.sh that pulls latest framework from GitHub and applies non-destructive updates:
- Universal files overwritten: conventions/, templates/, phase docs, CLAUDE.md, Conventions.md
- Project-specific files NEVER touched: References.md, feature-tree.md, conventions/overrides/, protocols/, catalogs/, docs/, todo/
- conventions/overrides/ preserved when updating conventions/ (backup → overwrite → restore)
- Updates CLAUDE.md at both archetype/ and project root
- Appends to VERSION-LOG.md with date and commit hash

Also updated inject.sh to create VERSION-LOG.md at injection time with bootstrap commit hash.

Tested on game-test project: update detected changed files, applied cleanly, logged to VERSION-LOG.md.

### Step 23: Bootstrap and scaffold logging

VERSION-LOG.md only recorded injection and updates. Bootstrap and scaffold phases left no trace.

Fixed:
- ONBOARD.md Step 5: after bootstrap, AI logs to VERSION-LOG.md (date, type, tech stack, files generated with counts, conventions read, discovery answers, key decisions)
- SCAFFOLD.md: after scaffold, AI appends to VERSION-LOG.md (date, sessions, systems built with locations, systems skipped, deps, verification)

A future AI or developer can now trace exactly what happened during each phase without needing the original conversation.

### Step 24: Enforcer hardening (second external review)

Second review of CLAUDE.md identified 8 suggestions. Audited against existing implementation:
- Items 1-2 already implemented in Step 20 (confirm before coding, mid-task re-scan)
- 6 genuinely new items implemented:

1. "If uncertain, ask" moved to first rule (was last). Most important safety valve shouldn't be the last thing the AI reads.
2. Meta-rule added: "Never modify CLAUDE.md, Conventions.md, or convention docs without explicit permission." Prevents AI from weakening its own rules when blocked by a convention.
3. Codebase search rule: "Search the codebase before building." feature-tree.md is manually maintained and can be stale. The code is the source of truth for what exists.
4. Commit discipline: "Commit after every verified change." Convention #2 has this but the enforcer didn't mention it, so AIs made 15 changes with no rollback points.
5. Convention #0 expanded from one-liner to actionable: "check what exists → use it and configure, or build reusable."
6. Database/migration row added to Conventions.md lookup table.

Enforcement rules: 16 → 19. CLAUDE.md still under 50 lines.

## Key Decisions (Final - all 33 decisions across 24 steps)

1. CLAUDE.md is a lean routing table with enforcement rules. Zero project context.
2. Conventions are framework-agnostic. They describe WHAT and WHY, not HOW.
3. References.md is the primary per-project file. feature-tree.md and docs/ are also per-project.
4. Convention docs account for AI-era shifts in why conventions matter.
5. The framework bootstraps new projects AND onboards existing ones.
6. Convention examples are conceptual descriptions, not framework-specific code.
7. Documentation flows alongside code at every phase.
8. Reusability is the meta-convention (#0) that governs all others.
9. Every convention produces a reusable foundational system at project creation.
10. Bootstrap = applying #0 to each convention for the chosen framework.
11. The bootstrap interviews the user in plain English instead of asking for tech stack upfront.
12. The AI reads the user's experience level and adjusts infrastructure choices.
13. Dark mode and established UI library are non-negotiable.
14. Existing project bootstrap has 4 parts: scan (A), extract rules (B), cross-reference (C), migrate/audit docs (D).
15. Rule extraction preserves full original text, never summarizes.
16. Documentation discovery searches entire project, not just /docs/.
17. When discovery is ambiguous, AI asks instead of guessing.
18. All extraction/migration is non-destructive.
19. CLAUDE.md uses lazy loading - read only 2-4 relevant conventions per task.
20. Workflow gate forces AI to identify and read conventions before writing code.
21. Cross-session memory warning prevents drift.
22. Conventions.md is a lookup index with task-to-convention mapping.
23. CLAUDE.md at project root (auto-loaded). Everything else in archetype/.
24. AI states which conventions it read before first code output.
25. Bootstrap gate: if References.md doesn't exist, run bootstrap first.
26. Scope expansion: re-scan Conventions.md if task scope changes.
27. archetype/ stays permanently as the engine. Not deprecated.
28. Project runs from root. archetype/ is the updatable engine behind it.
29. update.sh handles non-destructive framework updates. VERSION-LOG.md is the audit trail.
30. Bootstrap and scaffold log what they did to VERSION-LOG.md for traceability.
31. "If uncertain, ask" is the first enforcement rule, not the last.
32. Meta-rule protects the rules: AI cannot modify CLAUDE.md or convention docs without permission.
33. AI must search the codebase (not just feature-tree) before building anything.

### Step 35: Scaffold round 2 fixes — validator hardening + preamble dedup

Trigger: agent v2 (19/20 systems built with guidance, 0 silently skipped). Residual gaps narrow and technical.

Implemented:
- Validator CI-migration regex rewritten (workflow_dispatch safe, push-to-main auto-migrate = FAIL)
- Validator pre-commit hook check
- Validator persisted-queries check (when mobile clients)
- Validator OpenTelemetry exporter check (when OTel in References)
- Preamble deduplicated to scaffolding/_preamble.md, referenced by all playbooks
- DataLoader categorical mention in SCAFFOLD-BACKEND Step 12 GraphQL

Deferred: rate-limit research-at-scaffold (over-prescriptive); Phases 3-4 (still untested).

### Step 34: Scaffold phase — first audit and fix

Trigger: first-ever Phase 2 agent test against realistic backend GraphQL profile. Agent built 20/20 systems but 6 were improvised (no SCAFFOLD step owned them). Identified 10 silent-failure risks.

Implemented:
- SCAFFOLD.md split by project shape (router + FRONTEND/BACKEND/MOBILE/PLATFORM variants)
- Explicit convention mapping per step (no more "read the corresponding convention doc")
- Operational verification gates per step (specific commands, specific exit criteria)
- scaffolding/RED-FLAGS.md (new) — 10 known silent-failure patterns
- scripts/validate-scaffold.sh (new) — machine-verifiable scaffold check
- "Smoke-test feature" requirement as scaffold exit gate
- References.md-to-scaffold handoff check as first step

### Step 33: Steering restructure — routing over content

Trigger: user audit realigned framework direction. Previous steps were optimizing for minimalism; correct goal is steering. Zero expirable content is a hard rule. Compress, don't delete. Automation over discipline.

Implemented:
- Purged expirable prices and vendor-as-answer specifics from ONBOARD.md and Convention #0
- Split ONBOARD.md into linear-flow (400 lines) + 3 focused docs (RED-FLAGS.md, EXISTING-PROJECT.md, LEARNING-PROJECTS.md) routed from it
- Added categorical routing for: mobile decision tree, compliance regimes, GraphQL patterns, audit-log vs app-log, language-agnostic lint rules
- Added scripts/validate-migration.sh — machine-verifiable gate for existing-project migrations (replaces AI-discipline defense against summarization)
- Generalized learning-project exception beyond Kubernetes to any enterprise-infra-as-learning-vehicle
- Adversarial: added "use whatever" no-stack-preference rule

Pivot: Phase 1 is now well-covered (4 rounds of audits + 6 surface tests). Next: test Scaffold phase against a v4 output.

### Step 32: Round 3 verification fixes — closing residual silent-wrong gaps

Trigger: 3 v3 verification agents confirmed Step 31 fixes fired correctly AND surfaced 8 smaller gaps. Applied silent-wrong > polish heuristic.

Implemented (all small edits):
1. PWA vs responsive clarified as distinct choices in ONBOARD Group 2 decision table
2. Proactive learning-project heuristic — AI asks in first reply when solo + enterprise-infra declared
3. Hard-block for vague-regulated at deploy gate — prevents launch without BAA when question stays open
4. Okta/SSO tenant ownership verification added to Red Flag Combinations
5. Priority-ranking fallback order when user refuses (compliance > data > auth > core flow > scale > polish)
6. Budget-vague handling (parallel to vague-regulated rule)
7. Convention #0 learning-project carve-out added to Wrong/Right example
8. references-platform.md polish: pricing caveat, git-init guidance, sector-tagged Configuration Checklist

### Step 31: Round 2 — autonomous-baseline fixes

Trigger: 4 more agent runs validated Step 30 works AND surfaced gaps where the autonomous outcome is still silently wrong. Core design heuristic: "if AI can do good autonomously, it can do great with guidance" — so fix what causes silent failures without human intervention, defer what produces suboptimal-but-steerable outcomes.

Implemented:
- references-platform.md template (platform-choice outcomes had no template)
- claude-settings.json split into injected/root variants (silent hook failure fixed)
- Vague sensitive-data answer → default-assume-regulated (legal safety)
- Mobile disambiguation rule (no more silent React Native pick)
- Blueprint → SimplePractice in convention #0 (accuracy)
- Learning-project discovery question (validates "I want to build X to learn" as a legitimate bypass)
- Step 4 platform branch (explicit instructions, not just one-liner redirect)

Agent-run verification after Step 30:
- Therapy-v2: cleanly reached platform choice (SimplePractice) without needing to override Step 2. Red Flag table fired. Convention #0 extended language gave cite-able grounds.
- Blog-v2: pushback count 0→3. Same K8s stack generated BUT with explicit "Justified Override" rationale in References.md, cost quantified ($150-300/mo), and VERSION-LOG record of informed-consent. Framework no longer silently complies.

### Step 30: Bootstrap audit fixes — closing Step 3's flanks

Trigger: 5-agent audit (3 plan + 2 real execution runs in /tmp/archetype-test-*). Framework scored 8/10 on bootstrap design but the real-run blog agent generated EKS+Redis+Postgres+ArgoCD for a 5-reader blog, and the therapy agent only barely recovered a platform-appropriate choice because Step 3 saved it.

Core insight: Step 3 research is the framework's strongest defense, but it has weak flanks. Four shipped patterns actively undermine it: the "Developer example" (ONBOARD:310-314) modeled capitulation; line 165 "AI respects preferences" was an unqualified compliance clause; Step 2 compliance branch routed to AWS regardless of scale/budget; convention #0 was cited for scale-fit but only covered intra-project reuse. Plus no scope-change handler and no red-flag-combination table.

Implemented:
- Rewrote the Developer example to show pushback-then-confirm pattern, not capitulation
- Step 2 compliance branch now defers to Step 3 platform research before custom-build path
- Line 165 qualified: AI respects preferences AND runs Step 3 anyway if mismatched
- Added scope-change handler: re-ask affected groups, re-run Step 3 when requirements mutate
- Extended convention #0 to cover external reusability (market platforms), correcting the miscitation
- Added Red Flag Combinations table (8 known-bad pairs) between Step 2 and Step 3

Design decision: no new templates or conventions. Edits to existing files only. Closing Step 3's flanks is higher leverage than inventing new mechanisms.

Deferred to a future session:
- references-platform.md template for "user chose a platform" outcome
- templates/claude-settings.json path variants (inject vs clone)
- Fullstack-in-one-folder template (Next.js, Remix, Rails, Django)
- Mobile disambiguation (native/PWA/responsive)
- Vague-answer fallback path
- Mid-discovery summary checkpoint
- Priority-forcing mechanism

### Step 29: Framework hardening — validator, session review, convention #25, hooks

Trigger: Plan audit identified framework scores 8/10 design, 6/10 battle-testing. Six-improvement plan surfaced. After critical review, trimmed to what actually delivers value without bloat.

Dropped from original plan:
- Convention dependency graph (mechanical make-work, adding `## Dependencies` to 32 files with no clear consumer)
- 5 of 7 proposed hooks (noise generators — doc-freshness, feature-tree-audit, post-edit-doc-check add signal-to-noise inversion)
- ESLint-specific lint rule implementations (violates framework language-agnostic design — belongs in a separate examples repo)
- Synthetic test project specs (user battle-testing via real webpage development beats simulated specs)

Implemented:
- scripts/validate-framework.sh — self-test: checks file paths, convention count consistency, #N references, required sections in convention docs, duplicate numbers. Auto-detects framework location (root or archetype/ subfolder).
- conventions/25-automated-enforcement.md — new convention. Principle: lint rules catch violations at write time. Rules: every convention that CAN be automated SHOULD be, lint blocks don't warn, format on save, never bypass hooks. Framework-agnostic (research notes cover ESLint, Ruff, golangci-lint, Roslyn).
- bootstrap/hooks/pre-destructive-warn.sh — Claude Code PreToolUse hook. Reads JSON from stdin, blocks rm -rf, git reset --hard, DROP TABLE, etc. Exit 2 with stderr message for blocking behavior per 2026 Claude Code hook spec.
- bootstrap/hooks/post-task-verify.sh — Claude Code Stop hook. Advisory checklist reminder (build, tests, feature-tree, docs, commit). Non-blocking.
- bootstrap/hooks/README.md — install guide for Claude Code, Cursor, other tools.
- templates/claude-settings.json — ready-to-copy Claude Code hook configuration.
- templates/session-review.md — 5-question template for periodic session audits (drift tracking, convention clarity, routing correctness).
- development/MAINTAIN.md — added Session Reviews section.

Research finding: hooks are actively used in 2026. Claude Code has 21 lifecycle events, PreToolUse is the only blocker, stdin receives JSON, exit code 2 blocks with context. Confirmed before building.

Design decision: trimmed plan from 21-29 hours to ~6-9 hours by cutting items with marginal value. Original plan scored itself design 8→10 and battle 6→9; trimmed version delivers 80% of the value at 25% of the cost. The framework does not need a dependency graph, 7 hooks, or synthetic test specs.

Convention count: 25 → 26.

Files changed:
- scripts/validate-framework.sh (new dir)
- bootstrap/hooks/pre-destructive-warn.sh, post-task-verify.sh, README.md (new dir)
- templates/claude-settings.json, session-review.md (new)
- templates/hooks-spec.md (updated: points to real scripts)
- conventions/25-automated-enforcement.md (new)
- Conventions.md (added #25, count 25→26)
- backend/Conventions.md (count 25→26)
- README.md (count 25→26, added #25 mention)
- development/MAINTAIN.md (Session Reviews section)
- update.sh (added scripts/ to UNIVERSAL_DIRS)
- inject.sh (added scripts to copy list)

## Still Open

- Version tags on archetype repo (v1.0 when framework stabilizes)
- PROMOTE.md for moving from archetype/ subfolder to project root integration
- Optional addon docs (i18n, observability)
- Backend project migration (frontend done, backend not yet)
- Scaffolding for existing projects (closing gaps, not building from scratch)

## Current Status

Framework is production-ready with active refinement.
- 25+ commits to the product (archetype repo)
- Tested: vibe coder bootstrap (bakery, todo), developer stacks (.NET, Node.js), real existing project (MobileMed frontend)
- Full migration on MobileMed frontend: rule extraction, doc migration/audit, code audit, TODO generation
- Deployed to 3 projects: MobileMed frontend, game-test, framework itself
- update.sh tested and working on game-test project
- VERSION-LOG.md tracks bootstrap, updates, and scaffold phases
- CLAUDE.md: ~50 lines, 21 enforcement rules. Conventions.md: ~70 lines. 25 universal + 7 backend convention docs. 4 phase docs.
- Factory has: PLAN.md (26 steps), CHANGELOG.md (17 entries), ARCHITECTURE.md, ROADMAP.md (7 phases)
- Next: version tags, PROMOTE.md, backend project migration
- Full migration on Development3/frontend-dashboard: rule extraction (30 files), doc migration/audit (44 files), code audit (4 TODO files), all docs updated
- Deployed to 3 projects: MobileMed frontend, game-test, and the framework itself
- CLAUDE.md: 44 lines with lazy loading, workflow gate, session memory, bootstrap gate, scope expansion
- Conventions.md: 66 lines with lookup index and task-to-convention mapping
- Next: version strategy, update mechanism, backend migration, promotion documentation
