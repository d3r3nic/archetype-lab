# Run R4: decision ledger with expiry, research protocol, and correction capture

## Decision to inform

Three related additions. (a) A decision ledger: every technical decision the AI makes is one line, its reason, the alternatives rejected, who decided (AI or user), and a re-check trigger or date, so the AI never re-researches a settled question and the user never re-answers one. (b) A research protocol: when the AI must research (at bootstrap, at a new capability, when a recorded decision expires), which sources count, how findings are recorded with a verified-on date, and which model tier does the sweep. (c) Correction capture: when the user corrects the AI in any project and the correction is general rather than project-specific, it is logged and proposed upstream to the framework factory, so the founder stops repeating themself across projects.

## What exists today (verify by reading)

- dist/conventions/16-documentation.md: ADRs with context, reasoning, trade-offs; a research-time ADR template choice.
- dist/templates/references-*.md: References.md records stack and some decisions (for example the #22 foundation decision).
- dist/bootstrap/ONBOARD.md Step 3 "Research Before Deciding (DO NOT SKIP)", and the rule "research the current mainstream, actively maintained choice, never pick from memory".
- dist/development/FRESHNESS.md: identify what must be current, gate reads and actions, make updates durable, detect missed changes, required proof in the consuming system.
- Every convention's "## Research Notes" opens with "Dated notes: anything named in this section is an example from the time of writing and expires. Verify current options at bootstrap."
- dist/development/MAINTAIN.md Mode 3 "Convention evolution (signal-triggered)" and "Convention Evolution signal-collection": the upstream path exists in principle but is manual.
- dist/conventions/19-steering.md: drift prevention, scope discipline.
- Factory intake today: the Claude session writes planning/STEP-N records in the factory repository; there is no inbox for candidates coming from projects.

## Questions to answer

1. Decision records: what durable practice exists (architecture decision records and their variants, decision logs in safety-critical engineering, "sunset" or review-by dates)? What fields have proven necessary and which bloat? Is a single ledger file better than per-decision files for an AI reader, and why?
2. Expiry: how should a decision carry a re-check condition (a date, a trigger such as "when the chosen library's major version changes", "when users exceed N", "before the profile is promoted"), and who evaluates it (the AI at session start via FRESHNESS.md, a script, the user)?
3. Research protocol: which sources count as authoritative for a technology decision (official documentation, release notes, security advisories, standards) versus weak (blogs, memory); how to record the search performed and the date so a later session can judge staleness; how a cheaper model tier can do the sweep and a stronger tier the decision; how to avoid re-researching a question whose ledger entry is still valid.
4. Correction capture: define "general correction" versus "project-specific correction" with examples; how the AI recognises it is being corrected; the local record; de-duplication against existing conventions (the 29 in dist/conventions and backend B1-B7); the upstream hand-off that needs no specific code forge (a file in the project that the factory session can read, an export command, or another host-agnostic mechanism); and the factory's intake and triage.
5. Anti-bloat: rules for keeping the ledger readable by an AI with limited context (dist/conventions/17-context.md): size limits, archiving of expired entries, summarisation.
6. Plain language for the rider: how the ledger is shown to a non-technical user (the "decisions I made for you" view) and what they are asked to confirm (only the four constitution escalation categories).
7. Mechanical checks: which of these can a script verify (every References.md decision has a reason and a re-check field; no expired decision without a re-check record; every correction entry classified)? Mark mechanical versus judgment.

## Extra output for this run

A "Tiny version" section: the smallest change (one to two days) that captures most of the value, versus the full design.
