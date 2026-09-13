# Development Roadmap

## Step 64: Decision authority and the operating profile

- [x] Record the two findings (convention 19 contradicts the founder's operating rule; discovery learns the rigor facts and discards them), the decisions, the file list, and the verification plan in [STEP-64-DECISION-AUTHORITY-AND-PROFILE.md](STEP-64-DECISION-AUTHORITY-AND-PROFILE.md) (designed 2026-09-12, informed by research/astra-2026-09-12/).
- [x] Ship conventions #29 and #30, the reconciled convention 19 and enforcer, templates/profile.md, the debt template fields, the bootstrap questions and generation, scripts/validate-profile.sh with scripts/test-profile.py (157 tests); prove every rule on planted violations (verified locally 2026-09-12).
- [x] Independent audit by two reviewers that did not write the change: the Opus agent approved the final revision after twenty-eight reports; the Codex reviewer's twenty-seven rounds were folded in through round twenty-six and its remaining findings are recorded as the parser's stated limit. Published to the product repository (archetype PR #4, revision 0e4b5f6a550ad163fbac33037734bf7f742ff335).
- [x] Downstream adoption observed: game repository at dad28cc (PR #3) on 0e4b5f6, PROFILE.md written from the template (isolated, ai-decides, owner-stated), validate-profile.sh clean in normal and strict mode, installed self-test passing (reported by the game session 2026-09-12, checked read-only).

Planned after Step 64, each with its design input in research/astra-2026-09-12/ and triggered by Step 64's publication: Step 65 decisions, research protocol, correction capture, factory intake; Step 66 AI economy and starters; Step 67 the unattended window; Step 68 proof of gate, cost ledger, exit paths, license.

## Step 63: The framework tells the truth about itself; playbooks under the timeless gate

- [x] Record the self-claims rule, the wider gate, the per-line dated zone, and the per-file remediation record in [STEP-63-SELF-HONEST-PLAYBOOKS.md](STEP-63-SELF-HONEST-PLAYBOOKS.md).
- [x] Ship scripts/validate-claims.sh (group 11) and the widened validate-timeless.sh; prove both on planted violations; remediate bootstrap/, scaffolding/, development/, and the meta document to zero findings (verified locally 2026-09-11).
- [x] Independent adversarial audit (FIX-FIRST, two fix commits, APPROVE), then publish dist to the product repository (archetype PR #3, revision 3e670a57118a9127f0e726b1cb6c5cd97a228120).
- [x] Downstream adoption observed: game repository at 8a515fb on 3e670a5, installed self-test passing with groups 10 and 11 clean (reported by the game session, checked read-only).
- [ ] Follow-ups: automated gate runs on the product repository; validate-develop.sh reads shared-client names from References.md; portable date handling in validate-maintain.sh; dated notice on factory research/.

## Step 62: Timeless conventions, enforced

- [x] Record the rule, the three staleness classes, the allowed zone, the allowlist policy, and the per-file remediation record in [STEP-62-TIMELESS-CONVENTIONS.md](STEP-62-TIMELESS-CONVENTIONS.md).
- [x] Ship scripts/validate-timeless.sh inside validate-framework.sh; prove it fails on planted violations in every class and passes on the remediated dist (verified locally 2026-09-11).
- [x] Independent adversarial audit (FIX-FIRST, two fix commits, then APPROVE), then publish dist to the product repository (archetype PR #2, revision 5c305ef590cc2787b9c3aaa81a2c7b6aff1e0d67).
- [x] Downstream adoption observed: game repository at 42592a7 on 5c305ef, installed self-test passing (reported by the game session, checked read-only).
- [ ] Follow-up steps: bring phase playbooks into the validator's scope; propose a shape-agnostic core with expirable profiles; make update.sh skip a no-op VERSION-LOG entry and drop the placeholder line (regression in test-entrypoints.py).

## Step 61: Shared task and freshness rulebook

- [x] Record central task/source/freshness contract and ownership in [STEP-61-TASK-RULEBOOK.md](STEP-61-TASK-RULEBOOK.md).
- [x] Publish generic protocols and managed entry-point distribution after verification and independent review (product 326a030; 14 regressions).
- [x] Verify downstream adoption preserves local guidance and records platform capabilities honestly (source-byte comparison and project-owned context binding).

The checkboxes below retain their historical phase status.

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
- [x] update.sh: pull latest, diff, apply non-destructive changes (tested on game-test)
- [x] VERSION-LOG.md: tracks bootstrap date, every update with commit hash
- [x] inject.sh creates VERSION-LOG.md at injection time
- [x] Bootstrap and scaffold phases log to VERSION-LOG.md
- [x] Enforcer hardened to 19 rules (Step 24)
- [ ] Version tags on archetype repo (v1.0 when framework stabilizes)
- [ ] PROMOTE.md: step-by-step for moving archetype/ to project root
- [ ] Backend-specific enforcement rules (separate from universal)

## Phase 7: Maintenance & Self-Improvement
- [ ] Process for adding new conventions
- [ ] Process for updating when new techniques emerge
- [ ] Convention dependency graph documentation
- [ ] Optional addon docs (A1 i18n, A2 Observability)
- [ ] Scaffolding for existing projects (closing gaps, not building from scratch)
- [ ] Backend project migration

## Phase 8: Customer Deployment & Onboarding Automation (triggered by Edgar — 2026-04-22)
- [ ] Step 52: one-command customer provisioning (spec at planning/STEP-52-DEPLOY-AUTOMATION.md)
  - [ ] GCP folder/project/IAM/API provisioning (2 IAM bindings non-obvious)
  - [ ] Billing modes: reseller (master account) + direct (customer's card)
  - [ ] GitHub repo creation from template + tarball vendoring
  - [ ] Cloudflare DNS automation (CNAME, proxy=false enforced)
  - [ ] Cloud Run deploy + domain mapping + cert polling
  - [ ] Cloud Build triggers per branch (main/staging/production → envs)
  - [ ] Secret Manager wiring
- [ ] Step 53: Local dashboard UI wrapping Step 52 (IAP-protected, Cloud Run service in `internal/` folder)
