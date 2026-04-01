# AI Development Framework

A layered knowledge system for AI-assisted software development.
Makes any AI coding assistant follow a consistent engineering philosophy across any language, framework, or project.

## Structure

```
ai-dev-framework/
├── planning/                          # Framework design (the blueprint)
│   ├── ARCHITECTURE.md                # Layer system design
│   └── ROADMAP.md                     # Development phases and progress
│
├── research/                          # What informed the design
│   ├── existing-rulesets/ANALYSIS.md  # Analysis of 5 existing CLAUDE.md files
│   └── industry/audit-findings.md     # External research + system prompt analysis
│
├── dist/                              # THE USABLE FRAMEWORK (clean output)
│   ├── CLAUDE.md                      # Layer 0: The enforcer
│   ├── Conventions.md                 # Layer 1: The DNA catalog
│   ├── conventions/                   # Layer 2: Individual convention docs
│   ├── References.md                  # Layer 3: Template (filled per project)
│   └── templates/                     # Starter templates for backend/frontend/mobile
│
└── README.md
```

planning/ = source code of the framework
dist/ = build output - what gets deployed into real projects

## Status

Phase 1: Foundation (in progress)
See planning/ROADMAP.md for full status.
