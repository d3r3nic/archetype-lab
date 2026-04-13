# AI Dev Framework - Development Rules

This is the framework that produces AI development rules for other projects.
You are working ON the framework, not WITH it.

## Before Any Work

1. Read planning/PLAN.md - full context of what we're building and why
2. Read planning/ARCHITECTURE.md - the layer system design
3. Read planning/ROADMAP.md - current phase and progress
4. Check what exists in dist/ - that's the output we're building toward

## Rules For Working On This Framework

1. Everything connects. Every file must reference what it relates to. A new AI picking this up should be able to trace from any file to the full picture.

2. planning/ is the source of truth. If a decision was made, it's in PLAN.md. If the design changed, ARCHITECTURE.md is updated. If progress happened, ROADMAP.md reflects it.

3. dist/ is the clean output. Never put planning notes, research, or drafts in dist/. Only finished, usable framework files go there.

4. Do not write convention content without the full convention catalog being approved first. We need the map before any territory.

5. Conventions must be framework-agnostic. If you're writing "in React, do X" inside a convention doc, it's wrong. That belongs in References.md templates.

6. Keep every layer lean. If a file is getting long, it's probably mixing layers. Split it.

7. Update ROADMAP.md after completing any phase or task.

8. When the plan evolves, update PLAN.md with the new decision and why. Do not overwrite history - append the evolution.

## File Map

```
CLAUDE.md              ← you are here (development rules for this project)
README.md              ← project overview
planning/
  PLAN.md              ← full discussion history, decisions, reasoning (Steps 1-21)
  ARCHITECTURE.md      ← layer system design + project structure after promotion
  ROADMAP.md           ← phases and progress tracking
  CHANGELOG.md         ← every improvement with date, trigger, and changes
  CONVENTIONS-CATALOG.md ← the convention catalog with cross-references
research/
  existing-rulesets/   ← analysis of 5 existing CLAUDE.md files
  industry/            ← external research and system prompt audit
dist/                  ← the usable framework (clean output, its own repo)
```

## When making changes to the framework

1. Make the change in dist/ (the product)
2. Document WHY in planning/PLAN.md (append a new step)
3. Log WHAT changed in planning/CHANGELOG.md (with date, trigger, changes)
4. Update planning/ROADMAP.md if phases changed
5. Update planning/ARCHITECTURE.md if the design changed
6. Commit dist/ to the archetype repo (github.com/d3r3nic/archetype)
7. Commit planning/ to the lab repo (github.com/d3r3nic/archetype-lab)
8. Sync to deployed projects (MobileMed frontend, game-test, others)
