## Decision this informs

**Add one project-level rigor record and trigger-linked deferrals, but make the record revisable and multidimensional.** Bootstrap should derive it from business facts, then reassess it when exposure, data, consequences, or commitments change. A “0–100% compromise” setting has no defensible denominator: missing authentication cannot be offset by excellent documentation. The brief also understates existing tailoring. Compliance already determines mandatory scaffold systems; platform scaffolding already records deferral triggers and compensating controls; validators already have warnings and conditional exemptions. The gap is a consistent, machine-readable contract connecting these decisions across the lifecycle. I verified those mechanisms in the [scaffold preamble](/Users/d3r3nic/Development2/ai-dev-framework/dist/scaffolding/_preamble.md:22), [platform scaffold](/Users/d3r3nic/Development2/ai-dev-framework/dist/scaffolding/SCAFFOLD-PLATFORM.md:70), and [maintenance validator](/Users/d3r3nic/Development2/ai-dev-framework/dist/scripts/validate-maintain.sh:47).

## Durable principles

| Principle | Failure prevented | Evidence |
|---|---|---|
| Classify consequences and exposure separately from project age. | A small prototype processing consequential data receives inadequate safeguards. | **Standard:** impact baselines support tailoring and additional domain requirements. [Control-baseline guidance](https://csrc.nist.gov/pubs/sp/800/53/b/upd1/final). |
| Tailor individual obligations, preserving their intent and traceability. | “POC mode” disables an entire security or testing convention. | **Standard:** security requirements can vary by application risk while retaining identifiable requirements and documented decisions. [Verification standard](https://raw.githubusercontent.com/OWASP/ASVS/master/5.0/en/0x03-What-is-the-ASVS.md). |
| Reclassification requires gap analysis before changed use. | Research software becomes operational software without revisiting earlier omissions. | **Institutional practice:** update plans, applicable requirements, and transition work when classification changes. [Reclassification guidance](https://swehb.nasa.gov/spaces/SWEHBVD/pages/102695406/SWE-021%2B-%2BTransition%2Bto%2Ba%2BHigher%2BClass). |
| Readiness is evidence about a particular service and operating context. | A generic checklist produces an unjustified “production-ready” badge. | **Industry practice:** readiness reviews use service-specific reliability requirements and operational analysis. [Readiness-review account](https://sre.google/sre-book/evolving-sre-engagement-model/). |
| Distinguish organizational maturity, launch readiness, and safety integrity. | An internal maturity score is presented as certification or evidence of safety. | **Standards and framework:** organizational capability and integrity requirements have different scopes. [Maturity framework](https://owaspsamm.org/about/), [integrity-level standard abstract](https://webstore.iec.ch/en/publication/89824). |
| An experiment needs a containment boundary and an explicit disposition. | Throwaway code acquires permanent users and responsibilities. | **Public-sector practice:** prototypes test risky assumptions, remain unavailable for public use, and may be discarded. [Prototype-stage guidance](https://www.gov.uk/service-manual/agile-delivery/how-the-alpha-phase-works). |

**My assessment:** consequence classification, documented tailoring, explicit exit criteria, and reassessment are durable. Exact tier names, maturity scores, tool inventories, and universal coverage percentages are implementation choices. Safety integrity levels offer useful reasoning about required risk reduction, but should not become generic application-quality labels. [Functional-safety standard abstract](https://webstore.iec.ch/en/publication/5519).

## Proposed contract for Archetype

The following is proposed wording and design, not existing enforcement.

**Selection and precedence**

> Derive the project’s assurance requirements from its current use and the proposed next action. Required controls are the union of the non-deferrable floor, stage baseline, applicable risk requirements, and existing commitments. A project label cannot remove an obligation arising from actual use.

Use simple stage defaults:

| Stage | Operating boundary | Verification emphasis |
|---|---|---|
| `isolated` | Private experiment, synthetic disposable data, no real external effects or operational reliance. | Verify the experiment’s claimed behavior, important failure paths, and containment. |
| `trial` | Controlled evaluation with identified participants and a workable fallback. | Add boundary, integration, permissions, and recovery checks for everything actually exposed. |
| `operational` | People depend on the result, records, availability, or real transactions. | Add evidence for operational targets, deployment recovery, data restoration, monitoring, and incident handling. |

Personal data, financial effects, safety consequences, legal duties, and customer commitments add requirements at **any** stage. An internal tool can be operational. A second developer adds collaboration obligations without automatically requiring enterprise infrastructure.

This taxonomy is my design proposal, not a mapping to an external certification.

**The non-deferrable floor**

“Non-deferrable” means whenever the protected asset or hazardous capability exists.

| Floor | Required outcome and justification |
|---|---|
| Secret and environment protection | Keep credentials out of source, logs, and public artifacts; restrict privileges and separate experiments from live systems. Prevent credential theft and accidental live actions. |
| Trust-boundary protection | Validate untrusted input; enforce identity and resource authorization where access is restricted. A public information page needs no invented login system. Prevent unauthorized access and execution. |
| Safe irreversible effects | Protect valuable records and external actions with appropriate recovery, confirmation, reconciliation, or duplicate-action prevention. Disposable synthetic data may be reset; irreplaceable user data may not be silently lost. |
| Responsible personal-data handling | Before collection, establish purpose, access, retention, and applicable deletion/export procedures. A verified manual procedure can suffice where obligations and volume permit. Erasure is not unconditional; retention exceptions must be respected. [Regulator guidance](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/individual-rights/individual-rights/right-to-erasure/). |
| Authorized reuse | Verify rights to use code, assets, and data for the intended activity; preserve required notices. Public availability does not establish unrestricted reuse rights. [Licensing guidance](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repository). |
| Honest completion and review | Verify shipped behavior, keep required checks passing, and retain independent audit before merge. A profile cannot relabel a failing test, known unsafe behavior, or placeholder as acceptable completion. This preserves the [founder constitution](/Users/d3r3nic/.claude/CLAUDE.md:11). |

The security floor is my proposed synthesis. Secure development guidance supports risk-based checks, protected development environments, and verified component reuse; it does not prescribe this exact list. [Secure-development guidance](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-218.pdf).

**Records and ownership**

Introduce project-owned `project-profile.json`, linked from `References.md`. A dedicated structured file is justified because applicability, unknown values, and transitions need strict parsing, independently of prose formatting.

Required fields:

- `schema_version`, `policy_revision`, project/environment scope, and stage.
- Facts about audience, data, external effects, dependence, contributors, and applicable obligations.
- Evidence source, observation time, and invalidation/review conditions for consequential facts.
- Trigger definitions and references to budget authorization.

Unknown is an explicit value, never equivalent to false. Stage is validated against the facts. Shared components inherit applicable obligations from their consumers unless isolation is demonstrated.

Extend `TECHNICAL-DEBT.md`, whose current [entry format lacks promotion fields](/Users/d3r3nic/Development2/ai-dev-framework/dist/templates/technical-debt.md:5), rather than introducing another deferral ledger. Add strictly parseable fields:

`Kind`, `Control-ID`, `Scope`, `Reason`, `Risk`, `Containment`, `Owner`, `Task-ID`, `Due-before`, `Review-by`, `Closure-evidence`.

Distinguish planned future capability from an actual shortcut. Small actionable defects should be fixed, not converted into future work.

`feature-tree.md` links to the debt IDs and records deferred status. `VERSION-LOG.md` records profile changes, rationale, evidence, and review. Neither duplicates the canonical decisions.

**Applicability and truthful results**

Register stable control IDs, applicability predicates, permitted deferrals, and evaluators in the engine. Project facts select rules; they cannot rewrite the floor.

| Result | Meaning |
|---|---|
| `PASS` | Applicable check executed and its acceptance criteria were met. |
| `FAIL` | Applicable check executed and failed. |
| `NOT_APPLICABLE` | An identified rule’s applicability predicate is false, supported by evidence. |
| `DEFERRED` | Policy permits postponement within the current boundary; a valid ledger entry names its deadline or trigger. |
| `UNVERIFIED` / `ERROR` | Evidence, capability, or execution is missing or unreliable. |

Separate applicability from execution internally. A declaration alone cannot produce `PASS`; missing detection cannot produce `NOT_APPLICABLE`. Provider-owned controls require evidence about responsibility and configuration.

A proposed `scripts/validate-profile.sh` should report action, environment, policy revision, evidence revision, and each result. Exit zero means **the named action is permitted under the evaluated conditions**, not universally production-ready. Failed or unverified required checks block that action. Deferred controls remain visible and are excluded from pass counts.

The current [self-claims validator](/Users/d3r3nic/Development2/ai-dev-framework/dist/scripts/validate-claims.sh:4) checks counts, paths, and convention references. It does not establish runtime enforcement.

**Promotion and detection**

Evaluate **current conditions plus the proposed change**. Waiting to detect the first completed transaction is too late.

| Before this boundary | Required response | Detection |
|---|---|---|
| First outside participant or public access | Reassess audience, exposure, support, and access controls; close linked deferrals. | **Mechanical:** invitation/deployment adapter evaluates target state. **Judgment:** what participants will actually do. |
| First real personal data | Establish applicable handling, access, retention, and rights procedures. | **Mechanical:** gate imports and data-source configuration. **Judgment:** inspect data categories; scanning alone cannot prove absence. |
| First real money movement or consequential external action | Verify authorization, failure recovery, duplicate prevention, and domain obligations. | **Mechanical:** gate activation of live capabilities. **Judgment:** consequence and obligation assessment. |
| Operational reliance or valuable records | Verify availability expectations, restoration, incident response, and fallback. | **Judgment:** business dependence; **mechanical:** require corresponding evidence before activation. |
| Second contributor/operator | Review access, reproducibility, ownership, and handoff. | **Mechanical:** membership event when available; otherwise verified declaration and review. |
| Regulated claim or customer commitment | Verify applicable evidence and scope before publication or commitment. | **Judgment:** claim review; **mechanical:** require its recorded approval at the publishing boundary. |

At session start, resume, relevant scope changes, and consequential actions, refresh affected facts. This extends the existing [freshness contract](/Users/d3r3nic/Development2/ai-dev-framework/dist/development/FRESHNESS.md:11).

A trigger becoming true makes linked unresolved work blocking. Renewal, `won’t-fix`, or changing the stage label cannot bypass it. Downgrading requires verified removal of exposure and review of surviving obligations.

The host must invoke checks at actual action boundaries. Session reminders provide an additional defense. Out-of-band changes require reconciliation; without an adapter, automatic prevention remains unimplemented.

**Mechanical acceptance checks**

The proposed validator can check:

- Schema validity, duplicate IDs, required fields, known controls, and resolved links.
- Valid deferrals, trigger evaluation, review expiry, and closure-evidence presence.
- Current-versus-target contradictions and forbidden floor deferrals.
- Evidence revision/freshness and required review records.
- Negative cases: missing profile, unknown data, stale evidence, triggered deferral, relabeling, malformed ledger, and unavailable detector.

**Judgment checks:** risk classification, adequacy of containment and tests, legal applicability, evidence quality, and whether technical debt will obstruct the next boundary. File existence proves none of these.

**Phase skipping**

Defer obligations within phases; retain their essential outcomes.

| Phase | Safe isolated-stage deferrals | Retain now |
|---|---|---|
| Bootstrap | Detailed research for unused capabilities; optional reminder hooks. | Purpose, experiment success criteria, operating boundary, relevant constraints, platform/reuse assessment, and project map. |
| Scaffold | Unused infrastructure, remote deployment, capacity engineering, operational dashboards, and future integration systems. | Required local foundations, protected configuration, reproducible verification, containment, and an integrated smoke test. |
| Develop | Unrequested features and tests for capabilities absent from the experiment. | Correct implemented behavior, relevant failure tests, reuse discipline, documentation, passing applicable checks, independent audit. |
| Maintain | Broad operational reviews while the experiment remains isolated and inactive. | Trigger review, incident response, dependency/security response, and an explicit resume or retirement condition. |

Every omission needs a control ID, reason, owner, and due-before trigger or justified non-applicability. “Later” is insufficient.

Some of this requires coordinated framework changes: bootstrap already permits [optional hooks](/Users/d3r3nic/Development2/ai-dev-framework/dist/bootstrap/ONBOARD.md:428), but development currently requires [completed scaffolding](/Users/d3r3nic/Development2/ai-dev-framework/dist/development/DEVELOP.md:5), and the scaffold preamble requires [end-to-end verification](/Users/d3r3nic/Development2/ai-dev-framework/dist/scaffolding/_preamble.md:30). A profile must not silently override these requirements.

**Running cost**

Record infrastructure ceiling and AI-spend envelope alongside the profile, preferably by reference to the cost ledger. Include currency, period, approved recurring commitments, metered allowance, and measured-versus-estimated status. Subscription capacity is a separate constraint. A budget field is not an enforced cap unless a verified mechanism stops expenditure. Insufficient budget changes scope or sequencing, not the safety floor.

## What the interview must ask the user, in plain words

Ask only unanswered questions, in small groups:

1. “What do you want this first version to prove or let someone accomplish?”
2. “Who will use it now, and who do you expect to invite next?”
3. “Will it use made-up information or real information? What information, and about whom?”
4. “Could it move money, send messages, change important records, or affect someone’s health, safety, work, or access to a service?”
5. “If it stopped working or gave a wrong answer, what would happen? Could people use another method?”
6. “Are we testing an idea we may discard, or preparing something people will depend on? Is there a promised date?”
7. “Where are the people and organizations involved, and what have you promised them about privacy, reliability, or handling their information?”
8. “What monthly running cost and additional AI spending are you comfortable authorizing?”

The AI determines architecture, tests, controls, and sequencing from these facts, repository evidence, and current primary-source research. The founder does not choose a legal regime or technical profile.

For vague answers, preserve uncertainty and default to the safe side. Recommend a contained synthetic-data experiment while unresolved facts block real exposure. This would refine the existing rule, which currently [halts scaffolding as well as deployment](/Users/d3r3nic/Development2/ai-dev-framework/dist/bootstrap/RED-FLAGS.md:44). Do not claim that expensive infrastructure resolves uncertainty.

## Risks and open questions

- The ledger could become a permission slip for broken work. Prevent this through immutable floor rules, target-state checks, and independent review.
- Missing external visibility limits detection. State which boundaries are enforced, manually reviewed, or unobservable.
- Prototype promotion may require replacement rather than incremental repair. Evaluate both before promising continuity.
- No researched source establishes a universal compromise percentage or proves this proposed taxonomy.
- Founder input is needed for business consequences, commitments, scope changes, and spending. Technical classification remains the AI’s responsibility.
- Full safety standards and project-specific legal obligations were not audited in this run.

## Tiny version

**Estimated one to two days, subject to implementation review:** add the structured profile, extend the existing debt template with trigger fields, and implement one local validator for boundary changes. Route bootstrap, session checks, and pre-deployment review to it. Prove missing facts, expired evidence, triggered deferrals, and invalid floor exemptions block the affected action.

Initially preserve existing verification gates. This captures explicit boundaries, durable deferrals, and honest promotion decisions without reclassifying every convention.

The full design adds shared control metadata, consistent phase-validator reporting, host action adapters, and external-state reconciliation. Claim automatic prevention only where those integrations have been tested.

## Research Notes

Dated notes: anything named in this section is an example from the time of writing and expires.

**Read on 2026-09-12.** Searches covered control-baseline tailoring, production readiness reviews, software reclassification, security verification levels, organizational maturity, prototype disposal, safety integrity, secure development, erasure, and licensing. Repository findings came from file reads; recommendations are my synthesis. No files were written and no commits were made. Runtime enforcement was not tested.

| Source examined | Durable category |
|---|---|
| [NIST SP 800-53B](https://csrc.nist.gov/pubs/sp/800/53/b/upd1/final) | Impact baselines and control tailoring. |
| [NIST SSDF 1.1](https://nvlpubs.nist.gov/nistpubs/SpecialPublications/NIST.SP.800-218.pdf) | Risk-based secure development and evidence. |
| [NASA SWE-021](https://swehb.nasa.gov/spaces/SWEHBVD/pages/102695406/SWE-021%2B-%2BTransition%2Bto%2Ba%2BHigher%2BClass) | Reclassification and transition obligations. |
| [Google SRE engagement model](https://sre.google/sre-book/evolving-sre-engagement-model/) | Service-specific operational readiness. |
| [OWASP ASVS 5.0](https://raw.githubusercontent.com/OWASP/ASVS/master/5.0/en/0x03-What-is-the-ASVS.md) | Traceable application-security requirements. |
| [OWASP SAMM](https://owaspsamm.org/about/) | Organizational security maturity. |
| [GOV.UK alpha guidance](https://www.gov.uk/service-manual/agile-delivery/how-the-alpha-phase-works) | Contained, disposable experimentation. |
| [IEC 61508-5](https://webstore.iec.ch/en/publication/5519), [ISO/IEC/IEEE 15026-3](https://webstore.iec.ch/en/publication/89824) | Safety and system integrity levels; public abstracts examined, full standards not reviewed. |
| [ICO erasure guidance](https://ico.org.uk/for-organisations/uk-gdpr-guidance-and-resources/individual-rights/individual-rights/right-to-erasure/) | Conditional data rights and retention exceptions. |
| [GitHub licensing documentation](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/licensing-a-repository) | Reuse permissions; repository-host documentation, not a hosting recommendation. |