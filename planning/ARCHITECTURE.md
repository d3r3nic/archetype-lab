# Framework Architecture

## What This Is

A layered knowledge system for AI-assisted development. It produces the right rules, conventions, project scaffolding, and ongoing documentation for any project, any language, any AI assistant.

The framework operates in two dimensions: layers (what documents exist) and phases (when they're created and used).

## Core Principles

1. CLAUDE.md is a lean routing table. Direct enforcement rules with redirects to convention docs. Catches the AI mid-mistake and sends it to the right document.
2. Reusability is the meta-convention (#0). It governs how every other convention is implemented. Everything built once, configured for context.
3. Conventions are framework-agnostic. They describe WHAT and WHY. The HOW lives in References.md per project.
4. Documentation flows alongside code at every phase. Not as an afterthought.

## The Meta-Convention

Convention #0: REUSABILITY & COMPOSITION

This is not a peer of the other conventions. It sits above them. It is the lens through which every convention is implemented.

When ANY convention is applied to a project, it produces a reusable foundational system:

```
#0 Reusability (the meta-convention)
 ├── applies to → Styling      → build ONE theme system
 ├── applies to → Errors       → build ONE error handling system
 ├── applies to → API          → build ONE API layer
 ├── applies to → Auth         → build ONE auth system
 ├── applies to → State        → build ONE state pattern
 ├── applies to → Components   → build ONE component foundation
 ├── applies to → Forms        → build ONE form system
 └── applies to → every convention → build it ONCE, configure for context
```

NOTE: This intentionally overrides the AI system prompt default of "don't design for hypothetical futures." In this framework, reusable systems are the standard.

## Layer Architecture

```
Layer 0: CLAUDE.md (THE ENFORCER / ROUTING TABLE)
  - 19 enforcement rules, each a direct "never do X" with a redirect to a convention doc
  - Starts with "If uncertain, ask" (most important safety valve, first not last)
  - Includes workflow gates: read conventions before coding, verify before committing, search code before building
  - Includes meta-rule: AI cannot modify its own rules without permission
  - Catches the AI mid-mistake and routes it to the right document
  - No project context. No tech stack. No examples.
  - Lives at project root (auto-loaded by Claude Code). archetype/ subfolder has everything else.

Layer 1: Conventions.md (THE DNA INDEX)
  - Index of all 23 conventions with one-line descriptions and links
  - Starts with #0 Reusability as the meta-convention
  - References the 4-phase model at the top
  - Framework-agnostic. Language-agnostic. Lean.

Layer 2: conventions/*.md (THE CONVENTION DOCS)
  - One file per convention (23 files)
  - Each doc covers:
    - PRINCIPLE: what this convention IS and WHY (framework-agnostic)
    - REUSABLE SYSTEM: what foundational system this convention produces
    - RULES: direct rules to follow
    - VIOLATIONS: what wrong looks like (so AI can self-check)
    - RIGHT vs WRONG: concrete examples labeled with framework (illustrative)
  - Examples may use a specific framework for clarity but are labeled as such
  - The actual implementation for a specific project lives in References.md

Layer 3: References.md (PER-PROJECT CONTEXT)
  - Generated at bootstrap (Phase 1)
  - Updated during scaffold (Phase 2) with actual paths
  - Contains:
    - Project scope and purpose
    - Tech stack
    - HOW each convention's reusable system is implemented in THIS framework
    - WHERE each system lives in the project folder structure
    - Commands (dev, test, build, deploy)
    - Critical lessons learned
    - Convention overrides (where this project deviates and why)
  - Same filename everywhere. Different content per project.

Layer 4: Per-Project Documentation (GENERATED OVER TIME)
  - feature-tree.md: living map of all systems and features, auto-maintained by hooks
  - docs/systems/*.md: one doc per foundational system (generated at scaffold)
  - docs/features/*.md: one doc per feature (generated during development)
  - These are referenced from References.md and feature-tree.md
```

## Phase Architecture

The framework operates in 4 phases. Each phase produces artifacts the next phase builds on.

```
Phase 1: BOOTSTRAP (one-time)
  Input:  framework files + tech stack decisions
  Output: References.md, feature-tree.md (initialized), docs/ directories
  Guide:  bootstrap/ONBOARD.md
  Paths:  new project (create from scratch) or existing project (scan and map)

Phase 2: SCAFFOLD (one-time)
  Input:  References.md + conventions
  Output: foundational systems (code) + docs/systems/*.md
  Guide:  scaffolding/SCAFFOLD.md
  Action: apply #0 to each convention → build one reusable system per convention
  Order:  git → structure/types → theme → errors → API → auth → routing →
          state → components → forms → testing → CI/CD

Phase 3: DEVELOP (ongoing)
  Input:  scaffolded project + feature requirements
  Output: features (code) + docs/features/*.md + updated feature-tree.md
  Guide:  development/DEVELOP.md
  Flow:   understand → plan → implement (using systems) → verify → document → commit

Phase 4: MAINTAIN (periodic)
  Input:  project state + feature-tree.md
  Output: audit results, updated docs, evolved conventions
  Guide:  development/MAINTAIN.md
  Action: audit feature tree, verify docs freshness, check convention compliance
```

## How It All Connects

```
AI starts session
  → reads CLAUDE.md (routing table - catches mistakes, redirects to docs)
  → reads Conventions.md (scans the DNA index, sees 4-phase reference)
  → reads References.md (understands THIS project's systems and locations)
  → reads feature-tree.md (understands what exists, what's in progress)
  → AI encounters a task
  → CLAUDE.md rule catches if AI is about to violate a convention
  → redirect points to the convention doc (WHAT principle applies)
  → References.md says HOW the system works here and WHERE it lives
  → docs/systems/ has detailed usage for the specific system
  → AI implements using the existing system, never building ad-hoc

New AI agent joins mid-project:
  1. CLAUDE.md → learn the rules
  2. Conventions.md → scan the convention index
  3. References.md → understand this project's tech stack and systems
  4. feature-tree.md → see what exists and what's in progress
  5. docs/systems/ → understand the systems relevant to the task
  6. docs/features/ → understand the feature being worked on
  7. Start working
```

## Feature Tree

A living map of the project maintained by hooks and audited periodically.

```
feature-tree.md
├── Foundational Systems (status, location, docs link per system)
├── Features (name, routes, systems used, status, docs link)
└── Audit Log (date, scope, findings)
```

New AI agents read this first to understand the project's architecture. Hooks remind the AI to update it when features are added or modified. The audit script checks for drift between code and tree.

## Hooks

Hooks enforce rules deterministically (100% compliance). CLAUDE.md rules are advisory (~80% compliance). Critical rules should be hooks, not just CLAUDE.md lines.

- PostToolUse (Write/Edit): remind to update feature docs and feature-tree.md
- PostToolUse (TaskComplete): remind to run verification
- PreToolUse (Bash destructive): warn before destructive commands
- Audit script: compare feature directories against feature-tree.md entries

## Behavioral vs Enforcement Rules

These are separate concerns with separate homes:

- Enforcement rules (direct, catch mistakes): live in project CLAUDE.md
  "Never hardcode values." "Never scatter error handling."

- Behavioral rules (collaboration style, personal preferences): live in global ~/.claude/CLAUDE.md
  "Present options before implementing." "Wait for instructions."

Enforcement rules are universal across projects. Behavioral rules are personal to the developer.

## Convention Evolution

Conventions are stable. Implementation techniques change.
- New framework? Update References.md templates. Conventions unchanged.
- New AI technique proven? Update the relevant convention doc.
- New convention discovered? Add to Conventions.md index + create convention doc.

## AI-Era Convention Shifts

STAY (same rule, new reason):
- Reusability: AI duplicates more than humans. One source prevents drift.
- DRY: AI treats each copy as potentially canonical. Multiple copies = confusion.
- Folder structure: MORE important. It's how AI scopes its context window.
- Naming: AI infers patterns from names. Consistency matters more.

CHANGE (same name, different application):
- Comments: "explain INTENT and CONSTRAINTS" not "explain to humans"
- Documentation: machine-parseable specifications, not tutorials
- Code organization: optimized for context window, not human browsing
- File size: smaller files preferred (AI context management)

NEW (AI-era only):
- Verification loops (run tests after every change)
- Plan-before-implement (AI drifts without plans)
- Incremental changes (AI can't self-correct across large batches)
- Context management (keeping things scopeable for AI)

## Project Structure After Bootstrap + Promotion

After bootstrap and promotion, a project using the framework looks like this:

```
project/
├── CLAUDE.md              ← at root (auto-loaded by Claude Code)
├── References.md           ← at root (project context)
├── feature-tree.md         ← at root (living project map)
├── conventions/            ← promoted from archetype/ (read during development)
├── protocols/              ← promoted (project-specific workflows)
├── catalogs/               ← promoted (reference material)
│
├── archetype/              ← THE ENGINE (stays permanently)
│   ├── conventions/        ← source of truth for universal conventions
│   ├── bootstrap/          ← ran once, stays for reference
│   ├── scaffolding/        ← ran once, stays for reference
│   ├── development/        ← DEVELOP.md, MAINTAIN.md (ongoing reference)
│   ├── templates/          ← for generating new features/systems
│   ├── FRAMEWORK-SOURCE.md ← pointer back to framework repo
│   ├── VERSION-LOG.md      ← audit trail (bootstrap date, every update with commit hash)
│   ├── update.sh           ← pulls latest framework, applies non-destructive updates
│   └── inject.sh           ← original injection script (for reference)
│
├── docs/
└── src/
```

Key: the archetype/ folder is NOT deprecated after bootstrap. It stays as the updatable engine:
- Convention docs are read during development, not just bootstrap
- Templates are used when creating new features
- DEVELOP.md and MAINTAIN.md are ongoing reference
- update.sh needs the engine to compare versions and apply changes
- VERSION-LOG.md records every bootstrap, scaffold, and update with dates and commit hashes

The project runs FROM root (CLAUDE.md, References.md, conventions/). archetype/ powers it from behind.

## Update Mechanism (implemented)

update.sh handles non-destructive framework updates:
1. Pulls latest archetype from the framework repo (github.com/d3r3nic/archetype)
2. Shows what would change (diff between local and latest)
3. Asks for confirmation before applying
4. Overwrites universal files (conventions/, templates/, phase docs, CLAUDE.md) - framework-owned
5. Preserves project-specific files (References.md, feature-tree.md, overrides, protocols, catalogs, docs, todo) - project-owned
6. Preserves conventions/overrides/ when updating conventions/ (backup → overwrite → restore)
7. Updates CLAUDE.md at both archetype/ and project root
8. Appends to VERSION-LOG.md with date and commit hash

Tested on game-test project. Run from archetype/ folder: `./update.sh`

## What This Framework Is NOT

- Not a linter (linters handle formatting)
- Not a style guide (style guides are framework-specific)
- Not documentation (documentation describes what exists)
- Not a tutorial (tutorials teach how)

It IS an operating system for AI-assisted development.
