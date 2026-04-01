# Framework Architecture

## What This Is

A layered knowledge system for AI-assisted development.
A framework that produces the right rules, conventions, and project scaffolding for any project, any language, any AI assistant.

## Core Principles

1. CLAUDE.md stays lean. It is the enforcer - zero context, only behavioral rules and pointers. Bloated = missed rules.
2. Reusability is the meta-convention. It governs how every other convention gets implemented. Everything built once, configured for context.
3. Conventions are framework-agnostic. They describe WHAT and WHY, never HOW. The HOW lives in References.md per project.

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

This means:
- The AI never writes throw new Error() scattered across files
- The AI never creates a custom spinner per feature
- The AI never builds one-off API calls
- Instead, every feature plugs into the foundational systems

NOTE: This intentionally overrides the AI system prompt default of "don't design for hypothetical futures." In this framework, reusable systems are the standard.

## Layer Architecture

```
Layer 0: CLAUDE.md (THE ENFORCER)
  - 10-15 lines MAX
  - Behavioral rules only (how AI collaborates with the user)
  - Points to Conventions.md and References.md
  - No project context. No tech stack. No examples.
  - Never changes per project. Lives globally or is copied as-is.

Layer 1: Conventions.md (THE DNA CATALOG)
  - Index of all engineering conventions
  - Starts with #0 Reusability as the meta-convention
  - Each entry: convention name + one-line description + link to its doc
  - Framework-agnostic. Language-agnostic.
  - Lean. Just an index.

Layer 2: conventions/*.md (THE CONVENTION DOCS)
  - One file per convention
  - Each doc covers:
    - WHAT: the principle (framework-agnostic)
    - WHY: the reasoning (especially for AI-written code)
    - REUSABILITY: what foundational system this convention produces
    - VIOLATIONS: what wrong looks like (so AI can self-check)
  - Does NOT say HOW to implement (that's framework-specific, lives in Layer 3)

Layer 3: References.md (PER-PROJECT CONTEXT)
  - Generated when a project starts
  - Contains:
    - Project scope and purpose
    - Tech stack
    - HOW each convention's reusable system is implemented in THIS framework
    - WHERE each system lives in the project folder structure
    - Commands (dev, test, build, deploy)
    - Critical lessons learned
  - Same filename everywhere. Different content per project.

Layer 4: Deep Docs (ON-DEMAND, per project)
  - Generated as project evolves
  - Specific patterns, API docs, component catalogs
  - AI reads only when working on related features
  - Referenced from References.md
```

## How Layers Interact

```
AI starts session
  → reads CLAUDE.md (enforcer)
  → CLAUDE.md says "read Conventions.md"
  → AI scans Conventions.md (the DNA index, starting with #0 Reusability)
  → CLAUDE.md says "read References.md for project context"
  → AI reads References.md (understands THIS project's foundational systems)
  → AI encounters a task
  → Conventions.md points to the relevant convention doc
  → Convention doc says WHAT principle applies and WHAT reusable system should exist
  → References.md says HOW that system is implemented here and WHERE it lives
  → AI implements using the existing system, never building ad-hoc
```

## New Project Bootstrap

When starting a new project:
1. AI reads CLAUDE.md (enforcer) + Conventions.md (DNA)
2. User says "new [framework] project for [purpose]"
3. AI generates References.md applying each convention to the chosen framework
4. AI builds FOUNDATIONAL SYSTEMS by applying #0 to each convention:
   - Project structure (folders, aliases, barrels)
   - Theme/styling system (tokens, wrappers, scales)
   - Error handling system (boundaries, service, UI components)
   - API layer (configured client, interceptors, caching)
   - Auth system (token management, route protection)
   - State management setup (store pattern, slice conventions)
   - Component foundation (base components, wrappers around UI library)
   - Form system (validation, error display)
   - Unified loading/empty/error states
5. THEN feature work begins. Every feature plugs into these systems.

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
- Comments: not "explain to humans" but "explain INTENT and CONSTRAINTS"
- Documentation: not tutorials but machine-parseable specifications
- Code organization: optimized for context window, not human browsing
- File size: smaller files preferred (AI context management)

NEW (AI-era only):
- Verification loops (run tests after every change)
- Plan-before-implement (AI drifts without plans)
- Incremental changes (AI can't self-correct across large batches)
- Context management (keeping things scopeable for AI)

## What This Framework Is NOT

- Not a linter (linters handle formatting)
- Not a style guide (style guides are framework-specific)
- Not documentation (documentation describes what exists)
- Not a tutorial (tutorials teach how)

It IS an operating system for AI-assisted development.
