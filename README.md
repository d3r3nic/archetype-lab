# AI Development Framework (Archetype factory)

A layered knowledge system for AI-assisted software development.
Makes any AI coding assistant follow a consistent engineering philosophy across any language, framework, or project.

This repository is the factory: where framework changes are designed, recorded, and verified before they are published. The published framework lives in its own repository (github.com/d3r3nic/archetype) and is checked out here under `dist/`, which this repository ignores.

## Structure

```
ai-dev-framework/
├── CLAUDE.md                          # Rules for working ON the framework
├── planning/                          # Source of truth for every decision
│   ├── PLAN.md                        # Discussion history, decisions, reasoning
│   ├── ARCHITECTURE.md                # Layer system and phase model
│   ├── ROADMAP.md                     # Phases, steps, progress
│   ├── CHANGELOG.md                   # Every step with date, trigger, changes
│   ├── CONVENTIONS-CATALOG.md         # The convention catalog with sources
│   ├── BACKEND-AUDIT.md               # Backend convention research audit
│   └── STEP-*.md                      # Design and evidence records for recent steps
├── research/                          # What informed the design (dated, not remediated)
│   ├── existing-rulesets/             # Analysis of the founder's earlier rule sets
│   ├── industry/                      # External research and system-prompt audit
│   └── timeless-candidates-*.md       # Agent sweeps for further work
└── dist/                              # The published framework (separate repository)
```

planning/ = source code of the framework. dist/ = build output, published to real projects.

## Process

Design first in planning/ (a numbered step), then change dist/, then verify (`bash scripts/validate-framework.sh` and `python3 scripts/test-entrypoints.py` inside dist/), then an independent audit, then publish dist/ to the product repository, then record the published revision back here. See CLAUDE.md for the rules and planning/ROADMAP.md for status.
