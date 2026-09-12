## Decision this informs

Archetype should connect technical decisions, supporting research, and corrections through one traceable lifecycle. Amend two premises in the brief: record consequential choices rather than every implementation detail, and prevent unnecessary re-research rather than promise never to revisit a decision. I read the existing [ADR requirement](/Users/d3r3nic/Development2/ai-dev-framework/dist/conventions/16-documentation.md:20), [foundation-decision slot](/Users/d3r3nic/Development2/ai-dev-framework/dist/templates/references-frontend.md:117), and [freshness contract](/Users/d3r3nic/Development2/ai-dev-framework/dist/development/FRESHNESS.md:9). The platform template already goes further: its [decision log includes rationale, alternatives, and a volume-based review trigger](/Users/d3r3nic/Development2/ai-dev-framework/dist/templates/references-platform.md:42). Extend that precedent consistently. Correction promotion already has a [session-review and factory handoff](/Users/d3r3nic/Development2/ai-dev-framework/dist/development/MAINTAIN.md:39), but needs identifiable candidates and receipts. Also reconcile the [steering convention’s technical-choice approval requirements](/Users/d3r3nic/Development2/ai-dev-framework/dist/conventions/19-steering.md:16) with the founder constitution. Adding a ledger without fixing that conflict would preserve repeated technical questions.

## Durable principles

| Principle | Failure prevented | Evidence |
|---|---|---|
| Preserve the choice, its context, rejected alternatives, and consequences. | Future sessions blindly accept or reverse decisions. | **Industry practice:** [foundational ADR account](https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions) and [structured ADR template](https://adr.github.io/madr/). |
| Scale documentation to consequence and uncertainty. | Routine work becomes paperwork; consequential trade-offs disappear into one sentence. | **Engineering practice:** [decision-analysis guidance](https://www.nasa.gov/reference/6-8-decision-analysis/) records assumptions, alternatives, uncertainty, and rationale. **Own reasoning:** routine applications inherit an existing decision. |
| Review when assumptions change; preserve historical validity. | Calendar expiry causes needless replacement, or an old approval hides changed conditions. | **Standard guidance:** [component lifecycle practices](https://nvlpubs.nist.gov/nistpubs/specialpublications/nist.sp.800-218.pdf) call for reassessment under changed usage and ongoing maintenance checks. A universal review interval is **not established**. |
| Separate captured corrections from accepted shared rules. | One preference or mistaken diagnosis becomes universal instruction. | **Institutional practice:** the [reviewed lessons system](https://www.nasa.gov/nasa-lessons-learned/) preserves driving events and recommendations before incorporating lessons into practice. |
| Bound retrieval and research effort while retaining evidence. | Context overload, repeated searches, and cheap but unreliable conclusions. | **Papers:** [context-position experiments](https://arxiv.org/abs/2307.03172) and [model-cascade research](https://arxiv.org/abs/2305.05176). Applying these findings to Archetype requires project evaluation, not assumed savings. |

## Proposed contract for Archetype

The following is proposed wording and design, not existing enforcement.

**1. Record decisions once, with a short entry point.**

> Before making a consequential technical choice, locate an applicable recorded decision. Reuse it while its scope and prerequisites remain valid. Record a new decision when introducing or changing dependencies, boundaries, operational obligations, meaningful costs, or deliberate compromises. Routine implementation follows the governing decision.

Use the project’s existing ADR location when available. Otherwise introduce project-owned `DECISIONS.md`, linked from `References.md`. Start with compact structured records; move substantial records into project-owned `docs/decisions/` when necessary, preserving IDs and leaving a short index. This avoids two competing decision stores.

A single file is simpler to discover and maintain initially. Separate records permit selective loading and reduce concurrent editing conflicts as the project grows. The hybrid recommendation is **my design judgment**, not a demonstrated universal advantage for AI readers.

Required information:

| Field group | Required content |
|---|---|
| Identity | Stable ID, scope, decision date, lifecycle status. |
| Outcome | One-sentence choice and reason tied to the requirement. |
| Trade-off | Credible alternatives rejected and why; principal adverse consequence. |
| Authority | Decided by AI or user, responsible role, applicable authorization reference. |
| Evidence | Supporting source or research-record links, assumptions, verification observations. |
| Review | Review condition or deadline, evaluator, and observation source. |
| History | Rechecks and outcomes; replacement ID when superseded. |

Do not fabricate alternatives. A constrained choice can record why no viable alternative remained. Do not attribute a technical choice to the user merely because they approved its expense.

Keep full scoring matrices, stakeholder lists, transcripts, and extended analysis optional. Require them only when they resolve material uncertainty.

`References.md` retains current implementation facts and decision IDs. `feature-tree.md` links affected systems or features to those IDs. `VERSION-LOG.md` records adoption events. `TECHNICAL-DEBT.md` records actual compromises and their resolution obligations, rather than duplicating every decision.

**2. Treat review status separately from decision status.**

> A review deadline means that reliance needs reassessment. It does not erase the decision, prove the choice wrong, or authorize migration.

Keep lifecycle states such as proposed, accepted, superseded, and retired. Derive freshness separately: current, review required, or unverifiable.

Supported conditions should include:

| Condition | Evaluation |
|---|---|
| Review deadline reached | Compare an explicit timestamp against the current time. |
| Dependency changes materially | Compare the relevant dependency baseline; review compatibility and assumptions. |
| Usage exceeds the design envelope | Check a named metric, measurement window, and project-set threshold. |
| Project advances to a stricter operating profile | Evaluate before accepting that transition. |
| Support ends, a relevant advisory appears, or requirements change | Reassess affected assumptions when the event becomes known. |

Combine triggers with a review deadline where missed events matter. Trigger-only policies need an explicit rationale and an observable trigger.

At session start and resume, the AI checks the decision index and evaluates prerequisites relevant to its task. Before consequential actions, it repeats the necessary checks. Scripts evaluate dates and available structured observations; the AI evaluates meaning and alternatives. The founder does not monitor technical expiry.

A recheck records the triggering event, evidence actually observed, evaluator, outcome, and next review policy. Renewing a timestamp without a source observation is prohibited. Recheck outcomes are retain, supersede, retire, or unresolved.

Unresolved prerequisites block dependent work. Independent work continues. This extends the existing [affected-action freshness rule](/Users/d3r3nic/Development2/ai-dev-framework/dist/development/FRESHNESS.md:13).

**3. Research the unresolved question.**

> Research at bootstrap, before introducing an uncovered capability, and when relevant evidence expires or assumptions change. First inspect valid decisions and their evidence. Refresh the missing evidence before reopening the wider comparison.

Use authority appropriate to the claim:

- Official documentation and release notes for supported behavior and compatibility.
- Maintainer advisories and recognized vulnerability records for known security issues.
- Applicable standards for requirements.
- Official terms and pricing for contractual limits and costs.
- Reproducible project experiments for actual fit and performance.

Official documentation does not establish that its product is the best option. Independent primary measurements can challenge comparative claims. Blogs, discussions, search snippets, and model memory are discovery leads unless they provide directly attributable evidence. A practitioner’s original account can substantiate that practitioner’s experience.

Record research alongside its decision. Create a separate project-owned research note only when evidence is substantial or shared by several decisions. Include:

- Question, constraints, search queries, search date, and stopping condition.
- Sources actually opened, relevant sections, publication/revision information where available, and `verified_on`.
- Findings mapped to claims, contradictory evidence, experiments, and unresolved gaps.
- Researcher, model role used, decision owner, and resulting decision IDs.

Stop when evidence supports the requirements and resolves material uncertainty within the authorized budget. If evidence remains insufficient, preserve that gap. Bootstrap’s current [fallback to remembered knowledge](/Users/d3r3nic/Development2/ai-dev-framework/dist/bootstrap/ONBOARD.md:379) must not produce a “verified” decision.

When available and authorized, use a lower-cost model for bounded discovery, extraction, and comparison against explicit criteria. Use a stronger model for consequential synthesis, contradictions, and uncertain trade-offs. The deciding model must inspect decisive primary evidence itself. Choose actual models from current project evaluations and record them in project artifacts. If switching is unavailable, perform the bounded work directly and record that limitation.

**4. Capture corrections without automatically changing shared rules.**

Recognize corrections through explicit replacement instructions, rejected behavior, corrected facts, or explanations of a mistake. Do not require a particular phrase, and do not infer a universal rule from dissatisfaction alone.

| Correction | Classification and destination |
|---|---|
| “Use this customer’s approved colors.” | Project-specific requirement, recorded with its product context. |
| “Customer colors should come from configuration.” | General candidate; check whether an existing rule already covers it. |
| “That service no longer supports this capability.” | Factual correction; refresh affected research and decisions. |
| “Stop asking me to choose implementation tools.” | General workflow candidate, checked against existing authority rules. |
| “Keep this experimental integration for the demonstration.” | Project-specific exception with scope and review trigger. |

Extend the existing project-owned `docs/convention-evolution-log.md`, already [suggested by maintenance](/Users/d3r3nic/Development2/ai-dev-framework/dist/development/MAINTAIN.md:82), rather than invent another parallel log. Create it on the first relevant correction.

Each entry records an origin-qualified ID, date, concise original correction, mistaken behavior, intended behavior, classification, rationale, evidence pointer, local remedy, related decision/rule IDs, and disposition.

Search the [universal index](/Users/d3r3nic/Development2/ai-dev-framework/dist/Conventions.md:34), [backend index](/Users/d3r3nic/Development2/ai-dev-framework/dist/backend/Conventions.md:21), relevant playbooks, overrides, and existing candidates. Search synonyms and the underlying failure. An existing adequate rule points to a routing, enforcement, or obedience problem; it does not justify another convention.

One well-supported correction can become a candidate immediately. Repetition strengthens evidence but need not be a prerequisite.

**5. Give upstream handoffs durable receipts.**

Use a sanitized candidate file that an authorized factory session can read. File access is sufficient; no particular forge, messaging service, or export command is required.

Introduce factory-owned `planning/INTAKE.md` as the candidate register, linked from factory planning. Its purpose is triage before accepted work becomes a numbered factory step.

The handoff includes origin ID, candidate revision, proposed timeless principle, observed failure, related rules, and evidence safe to share. Private transcripts and customer details remain local.

The factory records:

1. Receipt keyed by origin ID and revision, making repeated imports idempotent.
2. Disposition: accepted, duplicate, project-local, rejected, or needs evidence, with rationale.
3. For accepted work: owning task, target layer, numbered step, and required verification.
4. Release reference after independent audit and publication.
5. Downstream adoption and verification that the corrected behavior holds.

Receipt, acceptance, release, and verified adoption are distinct states. A failed handoff remains pending. A pending candidate does not modify managed rules or expand permissions, consistent with the existing [task rulebook](/Users/d3r3nic/Development2/ai-dev-framework/dist/development/TASKS.md:31).

**6. Bound context without deleting unresolved obligations.**

Use the existing [project-set file-size policy](/Users/d3r3nic/Development2/ai-dev-framework/dist/conventions/17-context.md:9). Record a decision-summary budget and split policy in `References.md`; do not embed a universal line limit in the framework.

Show task-relevant accepted decisions and unresolved reviews first. Archive superseded or retired details while retaining searchable IDs, replacement links, and original evidence. An overdue decision still governing active code stays visible.

Summaries preserve scope, rationale, trade-off, review condition, and canonical pointer. They do not replace the source record or renew verification dates.

**7. State exactly what checks prove.**

| Proposed check | Classification |
|---|---|
| Required fields, valid dates, unique IDs, valid states, resolvable local links | **Mechanical** |
| Declared decision slots in `References.md` resolve to records containing rationale and review policy | **Mechanical** |
| Detect every technical decision hidden in arbitrary prose or code | **Judgment** |
| Determine deadline expiry and evaluate explicitly supported triggers | **Mechanical**, given trustworthy inputs |
| Reject dependent actions whose decision review remains unresolved | **Mechanical**, only through an implemented action boundary |
| Require recheck evidence references, outcomes, and chronology | **Mechanical**; whether the evidence justifies renewal is **judgment** |
| Require correction classification, disposition, and valid duplicate references | **Mechanical**; classification and semantic deduplication are **judgment** |
| Enforce project-set size limits and preserve archive links | **Mechanical**; summary fidelity is **judgment** |

Implement a proposed decision/correction validator and exercise missing fields, malformed dates, unresolved expiry, unavailable observations, duplicate receipts, and broken replacement links. Parse records individually; aggregate field counts cannot prove each record is complete.

The existing [maintenance age check concerns technical debt and emits warnings](/Users/d3r3nic/Development2/ai-dev-framework/dist/scripts/validate-maintain.sh:131). It is not decision-expiry enforcement. Documentation checks also cannot prove runtime blocking, as [FRESHNESS.md explicitly states](/Users/d3r3nic/Development2/ai-dev-framework/dist/development/FRESHNESS.md:33).

## What the interview must ask the user, in plain words

Reuse recorded answers. Ask only missing questions that affect the product:

- “What must this first version accomplish, and who will use it?”
- “What information must it protect, and what would be costly to lose?”
- “What ongoing spending limit should I work within?”
- “Is there a deadline or expected growth that changes what we need first?”

The AI chooses technologies, research depth, review conditions, record layout, and model routing from those constraints.

The “decisions I made for you” view should show outcome, reason, meaningful compromise, cost impact, and when it will be reconsidered. For example: “I kept the initial system simple to reduce upkeep. I’ll reassess it before the public launch.”

Seek confirmation only for new recurring spend, external commitments, actions on live customer environments, or changes to what the product is. Preserve existing authorization and present one plain-language recommendation. Silence does not create authorization.

## Risks and open questions

- Capturing a correction can misidentify its cause. Preserve evidence and review the proposed generalization.
- A current ledger can still omit a relevant dependency. Dependency completeness requires review.
- Unavailable source monitoring limits trigger detection. Record capabilities honestly; dormant projects cannot claim continuous checks.
- Candidate exports can expose private information. Share only authorized, necessary evidence.
- No research here establishes universal expiry intervals or model-routing savings for this workload.
- Founder input is needed only where business constraints or sharing authorization are missing.

## Tiny version

**Estimated one to two days, not measured:** standardize compact decision records in the existing project decision location; add evidence dates and review conditions; add correction entries to the existing evolution-log pattern; define factory file intake; reconcile technical-choice approval wording; implement structural/date checks with failure fixtures.

Route this through documentation, bootstrap, freshness, and maintenance. No new numbered convention is necessary.

The full design adds selective record extraction, structured trigger adapters, action-boundary enforcement, idempotent intake automation, release/adoption receipts, and evaluated model routing. Those capabilities require separate implementation evidence.

## Research Notes

Dated notes: anything named in this section is an example from the time of writing and expires.

Sources fetched and read on **2026-09-12**:

- [Michael Nygard, Documenting Architecture Decisions](https://www.cognitect.com/blog/2011/11/15/documenting-architecture-decisions): foundational ADR practice.
- [MADR documentation and template](https://adr.github.io/madr/): structured decision-record format, including optional confirmation and revisit information.
- [NASA Decision Analysis](https://www.nasa.gov/reference/6-8-decision-analysis/): engineering rationale and uncertainty capture.
- [NASA Lessons Learned](https://www.nasa.gov/nasa-lessons-learned/): reviewed organizational learning.
- [NIST SSDF 1.1, PW.4.1 and PW.4.4](https://nvlpubs.nist.gov/nistpubs/specialpublications/nist.sp.800-218.pdf): component evaluation and lifecycle reassessment.
- [FrugalGPT](https://arxiv.org/abs/2305.05176): model-cascade research; abstract read, no implementation benchmark performed.
- [Lost in the Middle](https://arxiv.org/abs/2307.03172): context-retrieval research; abstract read, no contemporary-model replication performed.
- [One Size Fits All? ADR template comparison](https://arxiv.org/abs/2604.27333): abstract describes expert screening and an undergraduate experiment. It does not establish the best file layout for AI readers.

Searches covered ADR rationale and supersession, MADR fields, engineering decision logs, reviewed lessons, component lifecycle checks, model cascades, and ADR-template comparisons. Additional searches surfaced AWS and Microsoft architecture guidance; those search results were not used as evidence. No decision-management product was evaluated.

Repository findings above come from file reads and targeted searches. No candidate inbox filename appeared in the inspected `planning/`, `research/`, or `dist/` trees; private or external intake remains unverified. No files were edited and no commits were made.