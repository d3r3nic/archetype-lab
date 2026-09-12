## Decision this informs

**Add an opt-in unattended work contract as an extension of the existing task protocol.** It should execute a finite queue prepared during interactive work, inside enforced limits, and leave verified changes awaiting independent review.

The brief needs three corrections:

- Scheduling is already mentioned in [FRESHNESS.md:29](/Users/d3r3nic/Development2/ai-dev-framework/dist/development/FRESHNESS.md:29) and [MAINTAIN.md:7](/Users/d3r3nic/Development2/ai-dev-framework/dist/development/MAINTAIN.md:7). My distribution-wide search found no dedicated unattended work contract.
- [task-context.md:3](/Users/d3r3nic/Development2/ai-dev-framework/dist/templates/task-context.md:3) is a **project binding**, not a per-task context file.
- “Anything that costs money” is too broad. Distinguish consumption within an authorized allowance from new charges or commitments. Likewise, a token cap alone cannot guarantee that the founder retains morning access.

## Durable principles

These are proposed transfers from operational practice, not evidence that unattended AI development is proven safe.

| Principle | Failure prevented | Evidence and transfer |
|---|---|---|
| Automate known, bounded procedures | Turning spare capacity into speculative work | **Industry practice:** automation benefits from well-scoped procedures and maintained preconditions. **Inference:** use a prepared queue and stop when empty. [Operational automation](https://sre.google/sre-book/automation-at-google/) |
| Treat the window as permission to prepare changes | Assuming an absent owner creates a suitable deployment window | **Own reasoning:** drafting isolated changes does not require exposing customers to them. Customer operations remain outside this contract. |
| Bound effects independently of the worker | A mistaken command escaping its branch | **Security guidance:** restrict functionality and permissions, and enforce authorization outside the model. [Excessive agency guidance](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/) |
| Separate scheduling, cancellation, and termination | Disabling tomorrow’s run while tonight’s keeps running | **Official documentation:** schedule suspension can leave existing jobs running; duplicate and missed starts are possible. **Inference:** require an active-run kill switch and deduplication. [Scheduler semantics](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/) |
| Journal intent and reconcile before replay | Duplicate commits, repeated effects, or lost progress after interruption | **Industry practice:** stable request identity and parameter validation distinguish retries from new intent. **Inference:** orchestration must be repeat-safe even though regenerated code is not deterministic. [Safe retries](https://aws.amazon.com/builders-library/making-retries-safe-with-idempotent-APIs/) |
| Keep verification separate from release authority | Treating passing tests as sufficient authority to merge | **Industry practice:** tests do not expose every production defect. Independent audit is additionally binding under the [founder constitution:17](/Users/d3r3nic/.claude/CLAUDE.md:17). [Release verification limits](https://sre.google/workbook/canarying-releases/) |

Runbooks and dry runs transfer directly: rehearse admission, execution, cancellation, and recovery in a disposable environment before enabling recurrence. Two-person control transfers as separation between author and independent reviewer, without requiring the founder to supervise every edit.

## Proposed contract for Archetype

All additions and checks below are proposals, not existing enforcement.

**1. Placement and authority**

Add a focused `development/UNATTENDED.md` playbook, routed from the task rulebook, convention lookup, and optional onboarding question.

Reuse [TASKS.md:15](/Users/d3r3nic/Development2/ai-dev-framework/dist/development/TASKS.md:15), which already requires outcome, evidence, dependencies, scope, allowed actions, budget, ownership, and bounded execution.

Proposed wording:

> Unattended work requires recorded consent, a prepared task queue, and verified host controls. The window changes when authorized work may run; it does not enlarge its scope or permissions. The worker cannot change its own authorization, queue admission rules, verification requirements, or resource limits.

Record the project policy in `protocols/task-context.md`; reference it from `References.md`. Include consent provenance, applicable projects, recurrence, timezone, end time, review condition, exclusions, resource limits, cancellation mechanism, and verified host capabilities. Keep personal availability in access-controlled project context when the repository is public.

Reconcile [19-steering.md:17](/Users/d3r3nic/Development2/ai-dev-framework/dist/conventions/19-steering.md:17), which requires technical approvals, with the [binding constitution:3](/Users/d3r3nic/.claude/CLAUDE.md:3): the AI plans engineering work under existing authorization. The founder decides product outcomes and meaningful business constraints.

**2. Queue admission**

Use a filtered view of the canonical task source, not another backlog. The interactive session marks eligible task revisions ready and records:

- Outcome, acceptance criteria, explicit exclusions, dependencies, and priority.
- Authorized paths, operations, resources, and maximum change size.
- Base revision, relevant specification and rule revisions, and evidence freshness conditions.
- Required verification commands, expected results, and recovery procedure.
- Task budget, eligibility rationale, and review requirement.

The run freezes its candidate task revisions at launch. It may consume that queue, but cannot invent tasks, expand scope, or automatically admit discoveries. New findings remain proposals linked to the originating task.

Recheck ownership, cancellation, prerequisites, and task revisions before execution and resume. This follows [FRESHNESS.md:15](/Users/d3r3nic/Development2/ai-dev-framework/dist/development/FRESHNESS.md:15). A connected tracker outage does not authorize a replacement tracker.

**3. Eligibility**

Every eligible item must already be planned and fit the enforced envelope.

| Work | Eligibility and boundary |
|---|---|
| Boilerplate | Eligible from a selected, pinned starter with configuration decisions settled. No new architecture, infrastructure, or invented interface behavior. |
| Tests | Eligible for specified existing behavior, including edge cases. A disagreement between behavior and specification becomes a finding, not permission to change the product. |
| Documentation | Eligible for verified implementation facts and existing decisions. No invented capabilities, promises, compliance claims, or publication. |
| Mechanical refactors | Conditional: bounded paths, unchanged interfaces and behavior, adequate regression evidence. Formatting is easier to admit than broad restructuring. |
| Dependency updates | Conditional: exact candidates researched beforehand, compatibility and supply-chain checks, isolated installation and execution. A “patch” label is insufficient evidence of safety. |
| Migration files | Conditional: a settled design may be drafted and tested against disposable synthetic data. Never apply it to real customer data. |
| Deployments, releases, merges | Forbidden in this mode, including automatic preview deployments triggered indirectly by publishing a branch. |
| External messages, purchases, permissions, live customer actions | Forbidden. Canonical task and evidence writes require specifically authorized operations in the binding. |
| Unplanned improvements or product decisions | Forbidden. Record the finding and stop dependent work. |

Tests, builds, and dependency installation execute code. They therefore need the same containment as implementation commands. Official workflow guidance documents how untrusted code and shared execution environments can expose credentials or compromise other jobs. [Workflow security](https://docs.github.com/en/actions/reference/security/secure-use/)

**4. Enforced safety envelope**

> Run in an isolated workspace on a dedicated task branch. Keep production credentials, customer datasets, unrelated projects, privileged host interfaces, and unrestricted external writes inaccessible. Constrain network access to admitted needs. Branch separation alone is not containment.

Additional obligations:

- Run one task at a time. Enforce project ownership across overlapping runs and a shared reservation where projects consume the same allowance.
- Protect control policy, required gate definitions, and authoritative evidence from worker modification. Adding planned tests does not authorize deleting assertions or changing expected behavior to obtain a pass.
- Verify baseline gates before editing. Verify each change and bind results to the exact resulting content, commands, environment, and gate definitions. Subsequent edits invalidate affected evidence.
- Permit only verified implementation commits. Preserve incomplete work as explicitly unverified recovery material.
- Never merge unattended. Independent audit must cover the final change revision before the normal merge process proceeds.
- Stop on cancellation, ambiguous requirements, stale authority, scope expansion, containment failure, unverifiable prerequisites, or budget exhaustion.
- Stop after the same required gate fails on two unexpected verification attempts. An explicitly planned test-first failure is different. Restarting the model does not reset attempts or budgets.
- An unexpected failing gate blocks advancement. Never disable, weaken, or silently exclude a check.

For resources:

> Bound wall time, model consumption, tool consumption, and incremental charges separately. Reserve capacity for verification, shutdown, and reporting before starting work.

For each reliably measured allowance:

`remaining allowance ≥ reserved foreground use + shutdown reserve + maximum next-step consumption`

Include retries and outstanding requests. Reservations must account for other consumers sharing that allowance. If trustworthy metering or enforceable bounds are unavailable, do not claim a protected morning reserve. Use a separately authorized, enforceably capped allowance or leave unattended AI execution disabled.

Default to no additional billed spend. Never create subscriptions, enable overages, or change accounts to continue after a limit.

**5. Host capability contract**

| Capability | Requirement |
|---|---|
| Constrained execution and protected policy | Required, including code invoked by tests and installation. |
| Hard deadline and resource enforcement | Required outside the worker’s reasoning process. |
| Cancellation and active termination | Required. Stop child processes and prevent new actions; record uncertain in-flight outcomes. |
| Exclusive ownership and duplicate-start protection | Required, using mechanisms appropriate to the host’s scale. |
| Durable journal and recovery | Required outside conversation context and disposable workspace lifetime. |
| Scheduled launch | Required for automatic recurrence. Without it, support a manually started finite run with identical controls. |
| Presence detection and delivered notifications | Optional. Explicit cancellation and the recorded deadline remain authoritative. |

Record capability evidence and its verification date. Unsupported properties must remain visibly unavailable.

Use timezone-aware windows, record their resolved absolute boundaries, and enforce elapsed-time limits independently. Skip missed windows rather than starting a catch-up run during the owner’s working hours.

**6. Morning brief and machine companion**

Create a proposed run-artifact template. Store artifacts in the project’s declared evidence location, for example the proposed project-owned `docs/automation/runs/{run-id}/` directory:

- `events.jsonl`: versioned, append-only machine journal controlled by the supervisor.
- `brief.md`: a human-readable view generated from that journal.

These files are justified by interruption recovery and per-run reporting. They are evidence, not a second task authority.

Journal fields should include run/window IDs, timestamps, policy revision, task revisions, ownership, operation IDs, input and output revisions, verification evidence, attempts, consumption with measurement basis, stop reason, unresolved actions, review status, and the next bounded action.

Record intent before consequential operations and observed outcome afterward. After interruption, inspect actual state before replay. An uncertain operation stays uncertain until reconciled.

Suggested brief, **illustrative wording only**:

> **Prepared for review:** Added the agreed starter files and checks for existing sign-in behavior. Required checks passed on the saved changes. Independent review is pending.  
> **Skipped:** The dependency update no longer matched its prepared plan.  
> **Needs you:** Nothing. The next interactive session can resolve the engineering issue.  
> **Usage:** [duration], [measured allowance consumed], [additional charges or explicitly unavailable].  
> **Stopped because:** [queue finished, window ended, limit reached, or blocker].

The supervisor should produce a minimal brief even if the model crashes. After a complete host outage, the next session reconstructs the record and labels any evidence gap.

Use `VERSION-LOG.md` for enabling or changing the policy, with links. Update `feature-tree.md` for actual system or feature changes. Use `TECHNICAL-DEBT.md` only for genuine debt. Keep periodic framework-drift analysis in the existing [session-review.md:3](/Users/d3r3nic/Development2/ai-dev-framework/dist/templates/session-review.md:3).

**7. Mechanical checks versus judgment**

Propose `scripts/validate-unattended.sh` for artifact consistency and a separate host qualification suite.

| Check | Classification and limit |
|---|---|
| Consent, timezone, window, limits, capability evidence, and canonical-source reference exist | **Mechanical:** structure and validity, not whether consent was meaningfully obtained. |
| Every executed task references an admitted revision and matching scope | **Mechanical:** reference and path checks. **Judgment:** scope completeness and eligibility. |
| Every unattended implementation commit maps to a task and successful gate evidence for its content | **Mechanical:** compare revisions, command manifests, exit results, and protected records. |
| Policy and required gates remain unchanged | **Mechanical:** protected-file comparison. **Judgment:** semantic weakening inside ordinary code or tests. |
| Every started run has a terminal or explicitly interrupted record and a brief | **Mechanical:** reconcile against an independently maintained launch inventory. |
| Duplicate launches, cancellation, deadline, quota exhaustion, crashes, and stale ownership are handled correctly | **Mechanical runtime tests:** require an actual host implementation and injected failures. |
| Reviewer differs from author and verdict covers final revision | **Mechanical:** identity and revision linkage. **Judgment:** review independence and adequacy. |
| Tests establish the intended behavior; documentation and summary are truthful | **Judgment**, supported by executable evidence. |

Current [task routing validation:203](/Users/d3r3nic/Development2/ai-dev-framework/dist/scripts/validate-framework.sh:203) explicitly does not prove enforcement. The existing [verification hook:12](/Users/d3r3nic/Development2/ai-dev-framework/dist/bootstrap/hooks/post-task-verify.sh:12) prints a checklist and exits successfully; it is not a completion gate.

## What the interview must ask the user, in plain words

Ask once when this capability becomes relevant, retaining existing answers:

1. “May I work on already-planned tasks while you’re away?”
2. “Which days and hours are you usually away, what timezone should I use, and when must I stop?”
3. “When you return, how much uninterrupted AI help do you normally need?”
4. “I’ll avoid additional charges. Is there an existing spending limit you want me to use?”
5. “Besides those limits, is there anything I must never do while you’re away?”

Record answers and consent provenance in the project policy. Revisit changed constraints, not every night.

The AI selects task sizes, isolation, verification, scheduling, models, and technical limits from project evidence and verified host capabilities. The founder’s expected morning use informs a conservative reserve; it cannot manufacture a quota guarantee.

## Risks and open questions

- **Host feasibility remains unverified.** This run inspected framework files and primary sources, not a deployed unattended runtime, scheduler, or account meter.
- **Review can become the bottleneck.** Cap queued output to the project’s demonstrated review capacity. An empty queue is a successful stopping point.
- **Evidence can be forged by the worker.** Worker-written “passed” fields are insufficient; protected execution records and independent review are necessary.
- **Availability patterns are private.** Public repositories should contain policy references, not personal schedules.
- **Founder input is limited to opt-in, availability, foreground needs, spending, and exclusions.** Engineering implementation is the AI’s responsibility. Silence during an away window does not authorize a new commitment.

## Tiny version

**One to two days is a planning estimate, conditional on an existing host supplying the required controls.**

Start with one project, one worker, one finite queue, local branches, no remote publication, and no dependency updates. Add the routed playbook, policy fields, compact journal/brief schema, and artifact validator. Qualify one host with cancellation, deadline, duplicate-start, budget, and crash-recovery tests.

If that host cannot enforce containment or allowance limits, ship the contract with execution disabled and use the queue interactively.

The full design adds recurring scheduling, coordinated reservations across projects, task-service recovery, richer evidence reconciliation, and carefully qualified dependency-update work. Both versions preserve the same verification and independent-review requirements.

## Research Notes

Dated notes: anything named in this section is an example from the time of writing and expires.

**Read on 2026-09-12:** the supplied briefs, personal constitution, applicable shared-memory guidance, planning documents, distribution entry points, task/freshness protocols, relevant conventions, templates, and hook/validator code. Product checkout revision: `3e670a57118a9127f0e726b1cb6c5cd97a228120`.

Local searches used ripgrep, including hidden and ignored distribution files while excluding repository metadata. Search terms included `unattended`, `overnight`, `scheduled`, `autonomous`, `AFK`, `sleep`, `kill.switch`, `budget`, and `loop`. Git supplied revision identifiers. These are text-search and version-control tools, not proposed runtime dependencies.

Web searches targeted operational automation, idempotent retries, scheduler concurrency/deadlines/suspension, excessive agency, and untrusted workflow execution. Only primary sources support the recommendations. All sources below were fetched on **2026-09-12**.

| Source or system examined | Durable category and relevance |
|---|---|
| [Google SRE, The Evolution of Automation](https://sre.google/sre-book/automation-at-google/) | Operational automation: bounded procedures, maintained assumptions, failed-fix handling. |
| [Google SRE, Canarying Releases](https://sre.google/workbook/canarying-releases/) | Release risk management: testing limitations and bounded exposure. Production canaries are not proposed for this mode. |
| [AWS Builders’ Library, Making retries safe with idempotent APIs](https://aws.amazon.com/builders-library/making-retries-safe-with-idempotent-APIs/) | Repeat-safe orchestration: request identity, reconciliation, changed-intent rejection. |
| [Kubernetes CronJob documentation](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/) | Scheduled execution: concurrency, missed starts, timezones, suspension, duplicate execution. Not a recommendation to install a cluster. |
| [OWASP, Excessive Agency](https://genai.owasp.org/llmrisk/llm062025-excessive-agency/) | Agent security guidance: constrained functionality, least privilege, external authorization enforcement. |
| [GitHub Actions, Secure use reference](https://docs.github.com/en/actions/reference/security/secure-use) | Workflow isolation and supply-chain security: untrusted execution, credentials, shared-state hazards. |
| Existing Claude Code hook adapters in the distribution | Host lifecycle integration. Inspected as repository code; current platform semantics were not independently tested. |

This was read-only research. No files were written, no commits were made, and no runtime safety guarantees were tested.