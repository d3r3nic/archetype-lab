# Run R3: the unattended work window

## Decision to inform

Should Archetype add a contract for unattended work: ask the user once for their sleeping or away hours, then let the AI run loops in that window on boilerplate, tests, documentation, and mechanical work, under a safety envelope, producing a plain-language morning brief. The founder's words: "asking the user for their sleeping hours or AFK hours where the AI can make use of loops to develop the boilerplate stuff".

## What exists today (verify by reading)

- dist/development/TASKS.md and dist/templates/task-context.md: task protocol and per-task context file.
- dist/templates/session-review.md: end-of-session review template.
- dist/templates/hooks-spec.md: host hooks, with a dated host section.
- dist/conventions/18-verification.md (run tests after every change, build gates), 19-steering.md (specs before code, drift prevention, breaking change protocol), 17-context.md.
- Founder constitution (binding on every session): escalate only new recurring spend, external commitments, actions on live customer environments, and changes to what the product is; no unverified claims; done means right; independent audit before merge.
- Nothing in dist/ mentions unattended, overnight, scheduled, or autonomous loops (confirm by search).

## Questions to answer

1. Durable practice for unattended automation acting on systems people depend on: change windows, blast radius limits, kill switches, two-person rules, runbooks, dry runs, idempotent steps, resumable journals. Which transfer to an AI developing software while its owner sleeps?
2. Eligibility: define precisely which work is eligible unattended (candidates: boilerplate from starters, tests for existing behaviour, documentation, mechanical refactors with gates, dependency updates with gates?) and which is forbidden (deployments, anything that costs money, external messages, schema changes on real data, product decisions, anything the interactive session has not already planned). Justify borderline cases.
3. Safety envelope: branch-only; hard token and wall-clock budget below the plan's allowance so the user is not locked out at breakfast; stop conditions (ambiguity, a gate failing twice, a decision that belongs to the user, budget threshold reached); never silence or weaken a gate to pass; evidence that every change passed the gates; the independent-audit-before-merge rule still holds, so nothing unattended merges itself. Anything missing?
4. The morning brief: structure and wording for a non-technical reader: what was done, what was skipped and why, what needs them, what it cost; and a machine-readable companion the next interactive session consumes. Where do both live (VERSION-LOG.md, session-review.md, a new file)?
5. Queueing: how does the interactive session leave work for the night (a queue of pre-planned, pre-scoped tasks with acceptance criteria) so the unattended loop never decides scope? Reuse dist/development/TASKS.md if it fits; say how.
6. Host-agnostic expression: the framework states the properties (a scheduled loop, a budget gauge, a kill switch, a journal); each host maps them. What properties are required versus optional, and what happens on a host that cannot schedule?
7. The interview: exact plain-language questions ("When are you usually away from the computer?", "Is it okay if I work then?", "Is there anything I must never do without you?") and how answers are recorded.
8. Mechanical checks: what a script can verify (a window is declared, every unattended commit references a queued task and a gate run, the brief exists for every window run). Mark mechanical versus judgment.

## Extra output for this run

A "Tiny version" section: the smallest change (one to two days) that captures most of the value, versus the full design.
