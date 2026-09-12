# Run R5: proof of gate, running-cost ledger, exit paths, and rider safety rails

## Decision to inform

Four smaller additions. (a) Proof of gate: require every project to hold durable evidence that its merge gate actually blocks a failing change, not a claim that a pipeline exists. (b) Running-cost ledger: record the expected monthly infrastructure cost at bootstrap per profile, actuals later, and the AI spend per feature, so cost efficiency is a number the user sees. (c) Exit path: for every managed service category the project adopts, a one-line exit plan at bootstrap (how data leaves, what would have to be rewritten, lock-in rating). (d) Rider safety rails: the short list of actions the AI never takes alone in any project at any profile, written for a non-technical owner.

## What exists today (verify by reading)

- dist/conventions/15-build-ci.md, 25-automated-enforcement.md, 18-verification.md: pipelines, lint, pre-commit, CI gates, build gates. None requires evidence that the gate is enforced at merge.
- dist/scripts/validate-*.sh run by hand; no CI in either framework repository runs them (recorded follow-up). A recent read-only audit of a downstream project found "nothing proves the automated checks block a merge", and the project's own notes admitted it.
- dist/bootstrap/ONBOARD.md asks about budget once (see RED-FLAGS.md "Vague budget answer"); nothing tracks cost afterwards.
- dist/bootstrap/ONBOARD.md Step 3 pushes managed platforms and services for non-technical users; nothing records how the project would leave one.
- dist/conventions/11-auth-security.md, 23-app-security.md, 24-authorization.md, backend/conventions B1-B7: security rules; none is phrased as an owner-facing "never without me" list.
- The product repository (dist/) has no license file.

## Questions to answer

1. Proof of gate: what minimal, durable, host-agnostic evidence shows a merge gate is enforced (an exported protection setting, a recorded failing-change test, a gate run log attached to the merge)? How often must it be refreshed, where is it stored (VERSION-LOG.md, a new evidence file), and what can a script check about it?
2. Running-cost ledger: what durable practice exists for cost allocation and showback in cloud operations that transfers to a small project; what fields (estimate at bootstrap, ceiling from the profile, actual per month, AI spend per feature); where it lives; how the AI obtains actuals without naming a vendor (the host or the project records them); what triggers a warning to the user in plain words.
3. Exit paths: what durable practice exists for vendor lock-in assessment and exit planning (including regulatory requirements in finance or public sector that demand exit strategies); the minimal per-service record (category, data export path, what must be rewritten, rating); when it is reviewed.
4. Rider safety rails: derive the list from the founder constitution's escalation categories and from irreversible or costly actions (deleting data, rotating or exposing secrets, pushing to a branch that deploys, spending money, sending anything to a customer, changing what the product is, weakening a gate). Write it in plain words for a non-technical owner and say how the AI enforces it on itself and how a hook or script can enforce part of it.
5. License: which licence categories fit a framework meant to be installed into private customer projects and a public product repository, and what must the founder decide (this is one of the few real founder decisions; phrase it as a one-line recommendation with the reason).
6. Mechanical checks for each of (a) to (d). Mark mechanical versus judgment.

## Extra output for this run

A "Tiny version" section: the smallest change (one to two days) that captures most of the value, versus the full design.
