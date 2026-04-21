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

5b. **Framework encodes character; project artifacts hold specifics.** The dist/ framework must not contain specific tool names as THE answer, specific API calls, version numbers, current pricing, or vendor product names as prescriptions. Those are specifics — they belong in bootstrapped project artifacts (References.md, feature-tree.md, conventions/overrides/, docs/features/, CLAUDE.md.additions), which are expirable and updated per project. Test: "if rolling back a specific tool mention leaves the direction UNCHANGED, it was a signal — keep it. If the AI now doesn't know what to do, it was a snippet — promote the intent, drop the snippet." Tools are the developer's toolbox; the framework teaches character.

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

## Battle-testing — downstream projects feed the factory

**Every template and product project built using this framework is a live battle test. Findings from those projects belong here — in this factory — as numbered Steps, NOT locally in the downstream project's own docs.**

Rules:

1. **Nothing discovered at a downstream layer stays only at that layer.** If a template project ran into a scaffolding gap, a convention ambiguity, a tooling pain, a new pattern worth codifying — it becomes a numbered Step in `planning/CHANGELOG.md` and a corresponding edit in `dist/`.

2. **The trigger line captures the finding.** Every Step's opening paragraph names WHAT downstream project surfaced it and WHAT they hit. Anyone reading CHANGELOG later knows which battle test produced which improvement. See Steps 44–49 for the pattern.

3. **The fix lands at the RIGHT layer.** Not every finding belongs in a convention. Some are phase-playbook updates (SCAFFOLD-FRONTEND.md etc.), some are script fixes (pulse-inspect, validators), some are template-file updates (references-*.md, feature-tree.md), some are new conventions. Pick the layer that matches the finding's character.

4. **Product-level UI/UX decisions do NOT promote up.** The factory governs character, not brand. If a customer site picked a specific color / font / iconography, that's product-local. Only the DISCIPLINE (token layering, wrapper boundary, every-state-designed) promotes up.

5. **After every downstream-triggered Step: push factory → push framework → pull into active projects.** The chain must be re-synced so the next battle test doesn't repeat a fix that's already landed.

6. **Check for un-promoted findings at the end of every session.** If the template accumulated new patterns, lint rules, tooling recipes, or workflow steps that aren't reflected in dist/, PAUSE feature work and promote them BEFORE continuing. Local-only rules become stale rules the moment context clears.

Test: could a fresh AI pick up archetype-lab + archetype repos, read only the framework + CHANGELOG, and build the same template the same way? If yes, the promotion is complete. If the AI would need to re-discover anything, something is still local.
