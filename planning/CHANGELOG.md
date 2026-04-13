# Framework Changelog

Every improvement to the Archetype framework, why it was made, and what triggered it.

## 2026-04-13 (Step 20-21)

Trigger: External review + real-world usage on MobileMed frontend

Changes:
- CLAUDE.md: moved to project root via inject.sh (Claude Code auto-reads from root)
- CLAUDE.md: added verification checkpoint ("state which conventions you read")
- CLAUDE.md: added bootstrap gate ("if References.md doesn't exist, run bootstrap first")
- CLAUDE.md: added scope expansion rule ("re-scan if task scope changes")
- hooks-spec.md: added setup instructions for Claude Code, Cursor, and other tools
- Architecture decision: archetype/ stays permanently as engine, not deprecated after bootstrap
- Project runs from root files. archetype/ is the updatable engine behind them.

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
