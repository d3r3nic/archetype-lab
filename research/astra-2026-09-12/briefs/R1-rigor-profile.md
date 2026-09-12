# Run R1: the rigor profile and the compromise dial

## Decision to inform

Should Archetype add a single project-level rigor profile, chosen once at bootstrap from plain questions, that governs which conventions are mandatory now versus deferred, how deep tests must go, how strict gates are, and how much technical debt is tolerated: "from 0 to 100 percent compromise in the case of a POC". And should it add a deferral ledger whose entries carry promotion triggers (for example: before the first outside user, before real personal data, before money moves, before a second developer joins, before a regulated claim is made), so a POC cannot silently become production.

## What exists today (verify by reading)

- dist/bootstrap/ONBOARD.md, Discovery Group 4 asks "personal project, a startup MVP, or an enterprise product" and the table "How to translate answers into technical decisions" maps that answer to a stack row only. Nothing downstream scales with it.
- dist/templates/technical-debt.md: append-only debt log with severity and status, no link to a profile, no promotion triggers.
- dist/development/MAINTAIN.md: tech-debt pruning policy, routine audit.
- Conventions treat every rule as mandatory. A few carry an "## Applies when" section (dist/conventions/26-pulse-monitor.md, 28-config-driven-content.md). dist/bootstrap/LEARNING-PROJECTS.md is the one recorded exception model.
- dist/bootstrap/RED-FLAGS.md: scale mismatches (solo user plus enterprise infra), vague answers default to the safe side.
- dist/scripts/validate-*.sh: checks either pass or fail; none reads a project-declared profile or says "not applicable at this profile".

## Questions to answer

1. What durable, evidence-backed ways exist to grade engineering rigor by risk and stage (production readiness reviews, launch gates, risk tiers, maturity levels, safety integrity levels, compliance readiness stages)? Which ideas survive across decades and which are fashion?
2. What must never be compromised even at the loosest profile (the floor): propose the list and justify each item (for example secrets handling, authentication, irreversible data loss, licensing, the ability to delete a user's data).
3. How should the profile be recorded so scripts can read it, and how should a validator express "this check does not apply at profile X" without lying (see dist/scripts/validate-claims.sh for the self-honesty rule)?
4. Promotion: what triggers move a project from one profile to the next, how is a deferred item attached to a trigger, and how is the trigger detected (by the AI at each session, by a validator, by the user)? How do you stop the quiet drift of a POC into production?
5. Phase skipping: the founder wants unnecessary phases skipped initially and planned over time if not blocking. Which parts of Archetype's four phases (bootstrap, scaffold, develop, maintain, in dist/bootstrap, dist/scaffolding, dist/development) are safely deferrable at a POC profile, and what record must exist so they are not forgotten?
6. Plain-language interview: the exact questions that let a non-technical founder pick the profile without knowing the words "profile", "rigor", or "technical debt". Include how vague answers are handled (the repo's pattern: default to the safe side).
7. Running cost: should the profile also carry a monthly infrastructure budget ceiling and an expected AI-spend envelope, and how would that be recorded? Keep this short; run R5 covers the cost ledger itself.

## Extra output for this run

A "Tiny version" section: the smallest change (one to two days of work) that captures most of the value, versus the full design.
