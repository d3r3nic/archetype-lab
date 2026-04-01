# Existing Ruleset Analysis

Source projects analyzed (2026-03-29):

1. /Users/d3r3nic/Development2/UNIRA-2025/unira (React Native/Expo)
2. /Users/d3r3nic/Development2/UNIRA-2025/unira-dashboard (React dashboard + hooks)
3. /Users/d3r3nic/Development2/UNIRA-2025/gmeblockchain-api (Node.js serverless)
4. /Users/d3r3nic/Development3/backend (TypeScript/SST)
5. /Users/d3r3nic/Development3/frontend-dashboard (React/MUI)

## Common Patterns Across All 5

Behavioral rules that appeared in every project:
- Boundary Respect (wait for instructions)
- Response Precision (answer what's asked)
- Preservation Principle (don't remove without permission)
- Audit Before Change (understand before modifying)
- No Workarounds (find root causes)

## What Worked

- Production-learned rules (e.g., claims.sub != database ID) have highest value
- Specific architectural rules (feature isolation, import discipline) prevent real drift
- Security rules needed because AI defaults are insufficient

## What Didn't Work

- 600+ line CLAUDE.md files: rules get ignored
- Advisory hook scripts (echo messages): unreliable compliance
- Inline code templates in CLAUDE.md: bloats the file
- Self-evident rules ("write clean code"): waste instruction budget
- Rules duplicating Claude Code system prompt: waste instruction budget

## Key Finding

Rules that tell AI WHAT to do (behavioral/architectural) work.
Rules that tell AI HOW to code (clean code, naming, patterns) are largely redundant with modern models.
The enforcer should focus on WHAT and WHERE, not HOW.

## System Prompt Overlap (Claude Code v2.1.87)

Already in system prompt (do NOT put in CLAUDE.md):
- Don't over-engineer
- Don't add features beyond what's asked
- Delete unused code
- Don't create unnecessary files
- Read before editing
- Watch for OWASP top 10
- Don't commit without asking
- Don't force push
