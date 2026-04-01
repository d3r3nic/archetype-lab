# Why These Rules: Audit Trail

## V2 Audit (2026-03-29)

Audited every rule against Claude Code's actual system prompt (v2.1.87) and Claude 4.6 model capabilities. Claude Code has a ~150-200 instruction budget. The system prompt uses ~50 slots. Every redundant rule wastes a slot and makes ALL rules less likely to be followed.

### Rules REMOVED (already in Claude Code system prompt)

These are literally baked into the system prompt or model training. Having them in CLAUDE.md wastes instruction budget:

REMOVED FROM CLEAN CODE RULE:
- "Remove unused imports, variables, dead code" - System prompt: "If something is unused, delete it completely"
- "Simple over complex" - System prompt: "Don't design for hypothetical future requirements"
- "No magic numbers" - Claude 4.6 does this by default

REMOVED FROM ANTI-PATTERNS:
- "Over-engineer" - System prompt: "Don't add features beyond what was asked," "don't create abstractions for one-time operations," "three similar lines > premature abstraction"
- "Add try/catch where framework handles" - System prompt: "Trust internal code and framework guarantees"
- "Skip input validation" - System prompt: "Only validate at system boundaries"
- "Write code files using bash/shell commands" (frontend) - System prompt: "Use Write/Edit tools"
- "Modify existing code without permission" - Duplicate of Rule 4

REMOVED FROM ERROR HANDLING:
- Merged into a note rather than a full rule. System prompt: "Don't add error handling for scenarios that can't happen"

REMOVED FROM SESSION START:
- "Read this file" - Claude Code auto-loads CLAUDE.md every session

TRIMMED FROM SECURITY:
- "Parameterize all database queries" - System prompt mentions SQL injection
- "Validate and sanitize all user input" - System prompt mentions input validation
- "Rate limiting" - Too specific for a master template
- KEPT: hardcoded secrets, presigned URLs, auth at service layer, object access checks (NOT in system prompt)

TRIMMED FROM INPUT VALIDATION:
- Removed "validate all external input" (in system prompt). Kept only the architectural WHERE (schemas live with feature).

TRIMMED FROM DOCUMENTATION:
- Removed "don't document what linters or types already enforce" (obvious)
- Removed "code comments explain WHY not WHAT" - Claude 4.6 does this well by default (kept anyway as it's brief and reinforcing)

### Rules ADDED (missing from V1)

7. PLAN BEFORE COMPLEX WORK
   Source: Research (Addy Osmani "waterfall in 15 minutes," Boris Cherny's plan-first workflow)
   Why: AI drifts on multi-file changes without a plan. This is the most validated workflow pattern.

10. STOP AND ASK
   Source: Was in Dev3/frontend-dashboard ("if something doesn't make sense, STOP and ask")
   Why: Prevents AI from building wrong things. User's point: "AI changes path quickly and requires steering."

11. SCOPE DISCIPLINE
   Source: System prompt covers this but needs reinforcement per user experience
   Why: Claude still drifts, especially when it sees adjacent code it wants to refactor.

12. FOLDER STRUCTURE (both BE and FE)
   Source: User explicitly stated: "it would create projects with zero folder structure"
   Why: Without this, AI creates files at root or in random locations.

18/20. DEPENDENCY AWARENESS
   Source: Was in unira CLAUDE.md ("always use npx expo install")
   Why: AI adds random dependencies without checking if functionality exists in current deps.

13 (FE). REUSABILITY FIRST - explicit override note
   Source: Dev3/frontend-dashboard factory-first philosophy
   Added note: "This intentionally overrides the default 'don't design for hypothetical futures'"
   Why: System prompt says the opposite. Without the note, Claude may ignore this rule because it conflicts with its training.

### Rules KEPT (still needed despite latest models)

BOUNDARY RESPECT, RESPONSE PRECISION, PRESERVATION PRINCIPLE, AUDIT BEFORE CHANGE:
- These are behavioral/collaboration style rules, not code quality rules
- Not in the system prompt
- Claude still over-anticipates and auto-implements without being told

NO WORKAROUNDS:
- Claude still suggests --force for dependency conflicts
- Not in system prompt

FEATURE ISOLATION:
- Architecture decision that cannot be inferred
- Without it, Claude imports across features

AUTHENTICATION:
- Production lesson from actual bugs
- Critical: wrong ID type causes silent data failures

SECURITY (hardcoded secrets, presigned URLs, auth layer, access checks):
- User confirmed: "if you don't tell it to consider security it would hardcode secrets"
- Research: 2.74x higher vulnerability rates in AI code
- System prompt has general OWASP mention but not these specific patterns

STYLING AND THEMING:
- User confirmed: Claude still hardcodes colors in 2026
- Not in system prompt

COMPONENT HIERARCHY:
- User confirmed: "zero folder structure" without this rule
- Not in system prompt

CONFIG-DRIVEN DESIGN:
- User's core architectural principle
- Not in system prompt (system prompt actually says the opposite: "don't design for hypothetical futures")

VERIFY YOUR WORK:
- Highest-value addition per Anthropic research (2-3x quality improvement)
- Not in system prompt (system prompt says "prioritize correct code" but doesn't say "run tests")

### Rule Counts

V1: Backend 18 rules, Frontend 18 rules
V2: Backend 21 rules, Frontend 22 rules
Net: Added 5 new rules, removed ~8 redundant sub-rules and anti-patterns

Despite having more numbered rules, V2 is actually shorter because the bloated Clean Code and Security rules were trimmed of redundant content.

## Sources

System Prompt Analysis:
- Piebald-AI/claude-code-system-prompts (v2.1.87, March 28, 2026)
- Claude Code Best Practices (Official Docs)
- Claude 4.6 Release Notes

Research:
- Anthropic 2026 Agentic Coding Trends Report
- Boris Cherny 100-Line CLAUDE.md Workflow
- Martin Fowler: Humans and Agents in Software Engineering Loops
- DORA 2025 Report
- Amazon Vibe Coding Incidents (4 Sev-1s in 90 days)
- CodeRabbit: AI vs Human Code Generation (1.7x more major issues)
- Tenzai: Security Study (69 vulnerabilities across 15 AI-built apps)
- METR Study: experienced devs 19% slower with unsupervised AI

User's Projects Analyzed:
- unira (React Native/Expo mobile app)
- unira-dashboard (React dashboard with hooks system)
- gmeblockchain-api (Node.js serverless backend)
- Development3/backend (TypeScript/SST backend, 23 rules)
- Development3/frontend-dashboard (React/MUI, factory-first architecture)
