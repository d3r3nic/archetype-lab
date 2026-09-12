# Astra research on the founder's vision for Archetype (2026-09-12)

Research note. Not a decision. Produced by five read-only runs of the founder's Codex CLI (model GPT-6 Astra, xhigh reasoning effort, live web search enabled) launched from the factory session on 2026-09-12 after the founder asked that Astra help research the next framework steps. Each run read the shared brief and one topic brief in `briefs/`, read this repository, searched the web, and returned one note. The notes in `notes/` are Astra's final messages copied verbatim, punctuation included. The briefs are the factory session's own words and carry the founder's vision as it was given.

The topics came from the factory session's gap assessment of the same day: a rigor profile with a compromise dial, an AI economy policy, an unattended work window, a decision ledger with research protocol and correction capture, and proof of gate with a cost ledger, exit paths, and rider safety rails.

## Factory session verification (what the factory session checked by hand)

- Every file-and-line citation the five notes make into this repository was opened (thirty-one citations across the five notes). All resolve to the content the note attributes to them. Examples: SCAFFOLD-BACKEND.md Step 17 does demand "open a PR with a typecheck failure, CI blocks the merge" and keeps no evidence of it; references-platform.md carries a "Decisions & Configuration Log" with reason and a volume trigger, a "Why This Platform" section, and "How to Leave the Platform Later"; MAINTAIN.md Mode 2 routes a framework-wide cause to a framework issue by hand; TASKS.md line 15 already requires an execution budget per task; task-context.md is a project binding, not a per-task file, as R3 corrected.
- Verified by search on the product checkout at 3e670a5: no shipped file mentions unattended work, model tiers, token budgets, or the founder's escalation categories; CREATING-FORKABLE-TEMPLATES.md has no inbound prose reference (only the timeless validator's regular expression names it); the product repository has no license file.
- Verified by reading: conventions/19-steering.md rules say "Get approval before implementing", "Present options before implementing ... wait for a decision", and "The human decides architecture". Three of the five notes flag this independently as a contradiction with the founder constitution (the prompter is the CTO; the AI decides everything technical; one recommendation, never a menu). The factory session agrees and treats it as the first thing to fix.
- Not verified: the web sources. Each note lists what it fetched and on which date; the factory session did not open them. Effort figures ("one to two days") are Astra's estimates and are labelled as such in every note.
- Usage, read from the run event logs: about 56k output tokens and 7.7M input tokens in total, of which 7.0M were cache reads. Wall time per run is in `timing.txt` alongside this file.

## Factory session reading of the five notes

Where the notes agree with each other without having seen each other:

1. Fix the authority contradiction first. Convention 19 hands architecture and implementation approval to the human; the constitution hands them to the AI and limits the owner's decisions to spend, commitments, live environments, and what the product is. A ledger, a profile, or a budget stacked on top of the old wording would keep the repeated technical questions the founder wants gone. The fix is a declared decision-authority setting per project (owner decides versus AI decides and records), with the constitution's escalation list as the default for a non-technical owner.
2. Extend the records the projects already have before inventing new ones: References.md for policy and current facts, TECHNICAL-DEBT.md with trigger fields for deferrals, VERSION-LOG.md for adoption events, feature-tree.md for links, the task binding for host capabilities, and the convention-evolution log the maintain playbook already suggests. New files only where strict parsing or volume demands them: a structured project profile, a compact decisions file (or the project's existing ADR location), a cost ledger, retained merge-gate evidence, and run journals for unattended work.
3. Declared is not enforced. Every note insists the framework say what a check proves. R1 proposes the result vocabulary PASS, FAIL, NOT_APPLICABLE, DEFERRED, UNVERIFIED, with the rule that a declaration alone cannot produce PASS and missing detection cannot produce NOT_APPLICABLE. R2 and R5 add that a budget field is not a cap and a label cannot remove an obligation that arises from actual use.
4. A floor that no profile may defer: secrets and environment protection, trust-boundary validation and authorization where access is restricted, safe irreversible effects, responsible personal-data handling before collection, authorized reuse of code and assets, and honest completion with independent review. R1 proposes it; R3 and R5 assume it.
5. The interview stays business-only. R1's eight questions (what the first version must prove, who uses it now and next, made-up or real information and about whom, could it move money or affect someone's health or access, what happens if it fails, disposable experiment or something people will depend on, what was promised about privacy and reliability, what monthly running cost and AI spend is comfortable) subsume the questions in the other four notes. Ask only the ones not already answered.
6. Host capability contract. The framework states properties (a scheduled loop, a budget gauge, a kill switch, a journal, a metered allowance); each host maps them; an unsupported property stays visibly unavailable rather than silently assumed.
7. Reuse before synthesis. Identical artifacts across projects of one shape belong in a maintained starter outside the engine; the scaffold clones or extracts and implements only verified gaps; the orphan forkable-templates guide becomes the routed source of that rule.

Where the factory session narrows or disagrees:

- R1's stage model (isolated, trial, operational) plus recorded facts (audience, data, external effects, dependence, contributors, obligations) is better than the tiered profile the brief proposed, and R1 is right that "0 to 100 percent compromise" has no denominator. Adopt stages plus facts plus floor; drop the percentage.
- R2's requirement to qualify every routing decision on representative tasks against a direct-execution baseline is correct and too heavy for a first release. The first release declares tiers as properties, records the tier per task, and requires stronger verification when the economical tier did the work. No learned router.
- R3's unattended contract should ship with execution disabled until a host qualification suite exists; R3 says the same in its tiny version. The queue and the brief are useful interactively before any scheduler exists.
- R4's hybrid decisions layout (compact file first, per-decision files later, one index) is accepted as design judgment, which is what R4 calls it.
- R5's license recommendation (a permissive license with an explicit patent grant) is a founder decision and is escalated as one line in the factory session's report.

## Proposed step plan

Recorded here for the roadmap; the design records decide.

- Step 64: decision authority and the rigor record. Constitution as a convention with a declared authority setting, convention 19 reconciled, structured project profile with stages, facts, and the floor, trigger fields in the debt template, a profile validator with the five-result vocabulary.
- Step 65: decisions, research, corrections. Compact decision records with review conditions, the research protocol, correction entries in the convention-evolution log, and a factory intake register with receipts.
- Step 66: AI economy and starters. The economy playbook, the forkable guide routed from bootstrap and scaffolding, budget and tier fields, session usage reporting with explicit unknowns.
- Step 67: the unattended window. The contract, queue admission, journal and brief, artifact validator, host qualification; execution disabled until a host passes.
- Step 68: proof of gate, cost ledger, exit paths, rider rails, license.

## Files

- `briefs/00-common-brief.md`: the shared brief every run read first (rules of the framework, the founder's words, the required note structure).
- `briefs/R1..R5-*.md`: the five topic briefs.
- `notes/R1..R5-*.md`: Astra's five notes, verbatim.
- `timing.txt`: start and finish times per run and the count of em dashes in each verbatim note.
