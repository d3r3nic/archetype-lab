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

## Key Decisions Made

1. CLAUDE.md is 10-15 lines MAX. Enforcer only. Zero context.
2. Conventions are framework-agnostic. They describe WHAT and WHY, never HOW.
3. References.md is the only per-project file. Same name, different content.
4. Convention docs account for AI-era shifts in why conventions matter.
5. The framework should bootstrap new projects, not just enforce existing ones.
6. No implementation techniques in convention docs - those change with frameworks.
7. Deep documentation is generated as projects evolve, not upfront.

## What We Have NOT Decided Yet

1. The complete list of conventions (we know reusability, theming, config-driven, boundaries - but the full catalog needs to be defined)
2. The exact format/template for convention docs
3. The exact format of the CLAUDE.md enforcer (we know it's lean but haven't written it)
4. The exact format of References.md template
5. How conventions interact with each other (dependencies, priorities)
6. Whether there should be backend/frontend/mobile variants of conventions or if they're truly universal
7. How to handle conventions that conflict with AI system prompts (e.g., reusability-first vs "don't design for hypothetical futures")
8. Version strategy - how to track changes to conventions over time

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

## Key Decisions Made

1. CLAUDE.md is 10-15 lines MAX. Enforcer only. Zero context.
2. Conventions are framework-agnostic. They describe WHAT and WHY, never HOW.
3. References.md is the only per-project file. Same name, different content.
4. Convention docs account for AI-era shifts in why conventions matter.
5. The framework should bootstrap new projects, not just enforce existing ones.
6. No implementation techniques in convention docs - those change with frameworks.
7. Deep documentation is generated as projects evolve, not upfront.
8. Reusability is the meta-convention (#0) that governs all others.
9. Every convention produces a reusable foundational system at project creation.
10. Bootstrap = applying #0 to each convention for the chosen framework.

## What We Have NOT Decided Yet

1. The exact template for convention docs (fields, structure)
2. The exact format of the CLAUDE.md enforcer
3. The exact format of References.md template
4. How conventions interact with each other (dependencies, priorities)
5. How to handle conventions that conflict with AI system prompts
6. Version strategy for convention evolution

## Next Step

Define the convention doc template. What fields must each convention doc have? This determines the structure before any content is written.
