# Development Roadmap

## Phase 1: Foundation (complete)
- [x] Define layer architecture
- [x] Create project scaffolding
- [x] Identify and catalog ALL conventions (v1: 33 → v2: cross-referenced → v3: merged to 25)
- [x] Elevate #0 Reusability as meta-convention
- [x] Define bootstrap protocol (#0 applied to each convention)
- [x] Define the convention doc template
- [x] Define the References.md template structure (frontend + backend + mobile)
- [x] Define the CLAUDE.md enforcer structure

## Phase 2: Convention Authoring (complete)
- [x] Write #0 Reusability (meta-convention doc)
- [x] Write all 22 core convention docs
- [x] Build the Conventions.md index (Layer 1)
- [x] Write CLAUDE.md enforcer (Layer 0)
- [x] Make all conventions framework-agnostic (descriptive sentences, no code)
- [x] Add multi-language examples to all conventions
- [x] Right vs Right examples in every convention

## Phase 3: Bootstrap & Discovery (complete)
- [x] Discovery interview flow for non-developers
- [x] Tech stack selection by AI based on user knowledge level
- [x] Infrastructure depth (managed vs self-managed) based on experience
- [x] Inject script for existing projects (inject.sh)
- [x] Vibe coder bootstrap test (bakery scenario)
- [x] Developer bootstrap test (.NET HR tool, Node.js todo app)

## Phase 4: Existing Project Migration (in progress)
- [x] Part A: Codebase scanning
- [x] Part B: Rule extraction (no summarization, full text preserved)
- [x] Part C: Cross-reference extracted files (CLAUDE.md, References.md, INDEX.md)
- [x] Part D: Documentation discovery, migration, and audit
- [x] Test on Development3/frontend-dashboard
- [x] Part D audit on frontend-dashboard (44 docs migrated, 41 audited, 15 doc issues fixed)
- [x] Code audit on frontend (4 TODO files with actionable fixes)
- [ ] Test on Development3/backend
- [ ] Promotion step: move from archetype/ subfolder to project root integration

## Phase 5: Real-World Validation
- [x] End-to-end test: bootstrap + scaffold + develop a working todo app
- [x] Verify scaffolded code follows conventions (centralized errors, API layer, shared UI)
- [x] Production-grade theme requirement (light + dark mode)
- [x] Established UI library requirement (don't reinvent buttons/inputs/modals)
- [x] Lazy loading, workflow gate, session memory, task mapping (external review)
- [x] CLAUDE.md at project root, bootstrap gate, scope expansion, hooks guidance
- [x] archetype/ as permanent engine (not deprecated after bootstrap)
- [ ] Run scaffolding on a Python backend
- [ ] Run scaffolding on a .NET backend
- [ ] Run scaffolding on a mobile project (React Native or Flutter)

## Phase 6: Versioning & Updates
- [x] CHANGELOG.md created with full history from origin to current
- [ ] Version tags on archetype repo (v1.0 when framework stabilizes)
- [ ] update.sh: pull latest framework, diff, apply non-destructive changes
- [ ] PROMOTE.md: step-by-step for moving archetype/ contents to project root

## Phase 7: Maintenance & Self-Improvement
- [ ] Process for adding new conventions
- [ ] Process for updating when new techniques emerge
- [ ] Convention dependency graph documentation
- [ ] Optional addon docs (A1 i18n, A2 Observability)
- [ ] Scaffolding for existing projects (closing gaps, not building from scratch)
- [ ] Backend project migration
