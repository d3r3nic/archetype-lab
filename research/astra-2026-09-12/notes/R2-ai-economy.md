## Decision this informs

Archetype should add an explicit AI economy policy, initially as a routed development playbook connected to existing conventions. Its objective should be **lower cost per verified outcome while preserving the founder’s interactive allowance**. A blanket “cheap model first” rule and a daily token counter would not achieve that reliably.

The repository changes the brief’s starting point:

- Execution budgets, bounded runs, and budget checks on retries already exist in [TASKS.md:15](/Users/d3r3nic/Development2/ai-dev-framework/dist/development/TASKS.md:15). The missing part is an operational accounting and enforcement contract.
- Context hygiene exists in [17-context.md:9](/Users/d3r3nic/Development2/ai-dev-framework/dist/conventions/17-context.md:9). Reuse exists in [00-reusability.md:17](/Users/d3r3nic/Development2/ai-dev-framework/dist/conventions/00-reusability.md:17).
- Scaffolding explicitly says subsequent projects can reuse the base in [SCAFFOLD.md:109](/Users/d3r3nic/Development2/ai-dev-framework/dist/scaffolding/SCAFFOLD.md:109). The forkable guide already requires boilerplate at source in [CREATING-FORKABLE-TEMPLATES.md:45](/Users/d3r3nic/Development2/ai-dev-framework/dist/development/CREATING-FORKABLE-TEMPLATES.md:45). I found no inbound Markdown reference to that guide within `dist/`.
- Pulse’s emitted contract contains project state and drift, without AI usage fields: [pulse-inspect.sh:354](/Users/d3r3nic/Development2/ai-dev-framework/dist/scripts/pulse-inspect.sh:354).

My search found no explicit model-tier, token-budget, cheap-first, or AI allowance policy. Application rate limits do exist. I did not measure whether downstream projects actually regenerate identical files.

## Durable principles

1. **Optimize accepted outcomes, including failed attempts.** Attribute execution, research, verification, retries, review, and shared preparation to work items. Show costs to the owner before introducing internal chargeback machinery. This prevents cheap requests from hiding expensive rework. **Evidence: industry practice**, [unit economics](https://www.finops.org/framework/capabilities/unit-economics/) and [cost allocation](https://framework.finops.org/framework/capabilities/allocation/).

2. **Separate money, allowance, context capacity, and elapsed time.** These are different constraints. A subscription’s remaining allowance cannot reliably be reconstructed from prompt length or an API-equivalent price. This prevents false “budget remaining” claims. **Evidence: current host documentation**, [usage accounting](https://learn.chatgpt.com/docs/pricing#what-are-the-usage-limits-for-my-plan).

3. **Protect foreground work through admission control.** Background work should start only when its bounded consumption preserves interactive capacity. Queue priorities and reservations carry over from batch scheduling. Reclaiming consumed allowance does not: cancelling a job cannot refund earlier inference. **Evidence: scheduling practice**, [backfill scheduling](https://slurm.schedmd.com/sched_config.html); **the allowance distinction is my reasoning**.

4. **A declared budget is not an enforced cap.** Notifications, dispatch prevention, and cancellation are separate capabilities. Name which exists. This prevents a dashboard from being mistaken for protection. **Evidence: operational documentation** distinguishing alerts-only budgets from service-specific spend caps: [budget controls](https://docs.cloud.google.com/billing/docs/how-to/budgets).

5. **Route only where success can be assessed credibly.** Coding research supports routing and cascading under particular conditions. One study’s repository experiment assumes reference tests provide accurate verification and measures cost through latency. That cannot establish universal monetary savings or production correctness. **Evidence: paper**, [routing and cascading](https://arxiv.org/html/2410.10347v3).

6. **Improve reusable inputs before adding orchestration.** A recent coding experiment found that its cheapest executor plus a prepared handoff matched the routed system on the studied benchmark. This supports testing simple baselines first. **Evidence: preliminary empirical paper**, [handoff experiment](https://arxiv.org/html/2608.04804v1). Extending that lesson to maintained starters is **my design inference**.

## Proposed contract for Archetype

Proposed core wording:

> Before spending AI capacity, identify the bounded outcome, reuse available artifacts, choose a qualified execution method, and reserve capacity for verification and the user’s interactive work. Charge retries and delegated work to the same budget. Stop before admitting work that exceeds a binding limit. Record observed usage and uncertainty. Budget pressure never lowers acceptance requirements.

Place the procedure in a proposed `development/AI-ECONOMY.md`, routed from session startup, the task workflow, bootstrap, and scaffolding. Add concise pointers from the reusability, context, and steering conventions.

**Budget and host binding**

Use `References.md` for project policy: priorities, approved spending, session and daily ceilings, foreground reserve, retry limits, and routing qualification evidence. Extend the existing project task binding for host capabilities; it already provides a permission and budget source in [task-context.md:16](/Users/d3r3nic/Development2/ai-dev-framework/dist/templates/task-context.md:16).

The host binding must identify:

- Shared account or allowance pool, including other projects and sessions.
- Every applicable window, its native unit, remaining amount, reset semantics, observation time, and measurement source.
- Actual model-selection, metering, reservation, cancellation, and scheduling capabilities.
- Whether protection is enforced, advisory, or unavailable.
- Extra-usage billing behavior and the authorization governing it.

For **every** binding constraint, admit a batch only when:

`observed use + outstanding reservations + new work bound + verification reserve + interactive reserve + uncertainty margin ≤ limit`

Reservations must be coordinated across consumers of the same pool. Independent project counters cannot protect a shared subscription.

A daily project ceiling supplements hourly, rolling, weekly, and model-specific windows. It never replaces them. Do not assume midnight or the founder going to sleep resets anything.

If consumption cannot be bounded or telemetry is stale, do not promise a hard cap. Pause unattended dispatch; allow only previously authorized bounded work under explicitly advisory controls. At a threshold, stop new requests and retries, checkpoint, and leave unfinished verification pending. Cancellation should stop AI work at a recoverable boundary, without terminating unrelated services.

**Routing classes**

“Economical” and “strong” describe demonstrated capability and total execution cost within the current host. They are not permanent model identities.

| Work class | Required executor properties | Acceptance and escalation |
|---|---|---|
| Mechanical edit | Exact transformations, bounded scope, reliable tool use; prefer a deterministic command where available | Inspect affected scope and run relevant existing checks. Escalate semantic ambiguity. |
| Test writing | Can implement independently specified behavior and boundary cases | Show the test detects the defect or relevant mutation. Stronger review defines ambiguous expectations. |
| Research sweep | Can retrieve primary sources and preserve dates, quotations, and links | Responsible researcher reopens decisive sources and resolves contradictions. |
| Summarization | Faithful extraction with traceable claims and preserved uncertainty | Check decisions, exclusions, and unresolved risks against originals before relaying. |
| Design | Strong reasoning across requirements, dependencies, failure modes, and lifecycle cost | Record tradeoffs; use targeted prototypes where uncertainty matters. |
| Security review | Adversarial reasoning about trust boundaries, authorization, and abuse | Combine executable checks with contextual review and reproducible findings. |
| Independent audit | Qualified reviewer separate from the author, with access to original requirements and evidence | Independent verdict remains required; a stronger model label alone proves neither independence nor correctness. |

Cheap execution must never supply its own sole acceptance authority. Better tests have exposed incorrect code and changed model rankings in [test-adequacy research](https://arxiv.org/abs/2305.01210).

Verification should scale with uncertainty and consequences. A deterministic rename needs scope and regression checks; an ambiguous authorization change needs strong contextual review regardless of who wrote it. Where extra review and likely escalation erase the savings, use the strong executor directly.

Qualify routes on representative project tasks using the actual tools and instructions. Record retries, escaped defects, acceptance rate, and total cost against a direct-execution baseline. Requalify after material changes to the model, host, or task mix. Confidence stated by the generating model is insufficient.

Preserve direct work as the default. Routing does not authorize agent fan-outs or override the founder’s existing delegation policy: [recorded instruction:13](/Users/d3r3nic/.claude/projects/-Users-d3r3nic/memory/feedback_workflows_opus_only.md:13).

**Starters instead of repeated synthesis**

Proposed rule:

> When a project shape repeatedly needs the same artifact after declared configuration is applied, maintain that artifact in a reusable source. Scaffold by consuming that source and implementing only the verified gaps.

The framework owns shape categories and acceptance properties. Dated starter repositories outside the engine own stack choices and implementation.

A starter should declare its immutable source revision, maintainer, compatible shape and capabilities, last verification evidence, permitted parameters, license, and update mechanism. Include an ownership boundary for shared and project-specific files. Exclude customer data, credentials, and inherited completion claims.

| Scaffold choice | Decision rule |
|---|---|
| Clone or extract | A maintained starter fits the required capabilities and passes verification independently of its source workspace. |
| Generate | No suitable starter exists, or the requirement is genuinely project-specific. Prefer a deterministic generator for repeatable variation. |
| Mix | Reuse the verified base, configure declared parameters, and implement only missing capabilities. |

A generator is itself reusable source; “generate” need not mean model-written files. Avoid importing unnecessary systems merely because a starter contains them.

Record selection and gaps in `References.md`, provenance in `VERSION-LOG.md`, and actual verified systems in `feature-tree.md`. Repeated corrective patches go back to the starter source. Existing forkability guidance already requires an independent verify chain and source fixes: [CREATING-FORKABLE-TEMPLATES.md:7](/Users/d3r3nic/Development2/ai-dev-framework/dist/development/CREATING-FORKABLE-TEMPLATES.md:7).

**Measurement and showback**

Use one canonical activity ledger attached to the existing task source. A local structured ledger is justified only where the project lacks that capability; declare its location through the task binding.

Record per attempt:

- Task, feature, session, and parent-run identifiers.
- Task class, requested and actual tier, routing-policy revision, and escalation reason.
- Start/end times, active execution time, waiting time, retries, and outcome evidence.
- Reported input/output usage, cache categories where available, and accounting semantics.
- Actual billed cost when available; otherwise a clearly labelled estimate and dated pricing basis.
- Allowance observations, reservations, stop reason, and telemetry completeness.

Do not sum overlapping token categories or treat current context size as cumulative usage. Aggregate children once, include failures, and put shared preparation into an explicit shared bucket. Unknown values remain unknown, never zero.

Feature documentation links to the ledger and acceptance evidence. `VERSION-LOG.md` records policy and starter changes, not every inference call. `TECHNICAL-DEBT.md` records actual shortcomings, such as missing metering, with an owner and resolution trigger.

Founder-facing showback should answer: **what finished, what remains, what was spent or consumed, how reliable the measurement is, and why work paused**. Claimed savings require a comparable baseline.

**Pulse extension**

The current spec requires a contract-version bump whenever the shape changes: [pulse-monitor-spec.md:63](/Users/d3r3nic/Development2/ai-dev-framework/dist/templates/pulse-monitor-spec.md:63). Therefore:

- Introduce the next contract version while preserving existing field meanings.
- Add an `aiActivity` object containing observation time, coverage, enforcement state, budget windows, recent outcomes, usage totals, retries, and pause reason.
- Update the collector, spec, and UI together. Verify old snapshots remain readable by the new UI; absent telemetry displays “unavailable.”
- Keep detailed records in the ledger. Pulse consumes a sanitized projection and remains read-only and development-only.

Its snapshot currently has no history, so it cannot become the accounting authority: [pulse-monitor-spec.md:152](/Users/d3r3nic/Development2/ai-dev-framework/dist/templates/pulse-monitor-spec.md:152).

**Checks**

These are proposed mechanisms, not claims about shipped enforcement.

| Check | Classification and limit |
|---|---|
| Policy fields, units, source links, and capability states are declared | **Mechanical:** schema validator. Cannot prove the values are true. |
| Every observed session has terminal or interrupted accounting | **Mechanical:** reconcile ledger against an independent host run inventory. Cannot detect unobserved hosts. |
| Duplicate events, child accounting, totals, and missing telemetry | **Mechanical:** ledger reconciliation. |
| Dispatch respects reservations, stale telemetry, cancellation, retries, and concurrent consumers | **Mechanical:** host integration tests with boundary and failure cases. |
| Starter provenance, allowed transformations, and clean extraction verification | **Mechanical:** manifest, artifact comparison, and isolated verification. A reference alone cannot prove reuse occurred. |
| Pulse compatibility and unavailable-data behavior | **Mechanical:** contract fixtures and renderer checks. |
| Route suitability, meaningful verification, starter fit, and claimed savings | **Judgment:** evidence review. |

## What the interview must ask the user, in plain words

Ask only for preferences not already known:

- “Should work wait when your existing allowance runs low, or is there an approved amount of extra spending?”
- “When do you need the assistant available, and is that for occasional questions or sustained work?”
- “What deadline matters, and which failures would be especially costly?”
- “May already-approved work run while you are away, and during which hours?”

The AI translates these into reservations, scheduling priority, bounded attempts, and eligible routes using observed consumption and current host capabilities. “Save money” permits slower delivery. It does not permit weaker security, skipped tests, or a reduced product scope.

## Risks and open questions

- Shared-account activity outside the host can defeat reservations. Protection must disclose its coverage.
- Cheaper execution can increase review work or silently pass inadequate tests.
- Starter maintenance and update ownership can cost more than reuse saves for uncommon shapes.
- Model changes and task drift can invalidate routing evidence.
- Telemetry must avoid storing prompts, secrets, or private source content unnecessarily.

Actual account limits, installed metering coverage, starter availability, and routing savings remain unverified. Founder decisions are limited to spending, availability preferences, product scope, and authorization for unattended work.

This run performed research only: no file writes, commits, model-routing experiments, or enforcement tests.

## Tiny version

**One to two days, engineering estimate:** connect the forkable guide to bootstrap and scaffolding; add the bounded-execution rule and project policy fields; record starter provenance; extend session reporting with measured usage or explicit unknowns; add structural validation. Use an existing host meter where available and label advisory protection honestly.

**Full design:** add coordinated reservations, qualified routing with rollback, complete attempt accounting, tested cancellation and resume behavior, and the versioned Pulse projection. Implement each when a consuming host can support and verify it. A learned router is justified only after measured simpler approaches leave material savings unrealized.

## Research Notes

Dated notes: anything named in this section is an example from the time of writing and expires.

All sources below were fetched and read on **2026-09-12**. Findings are source reports, not experiments reproduced during this run.

Searches included “LLM routing coding tasks cascade SWE bench model cost routing paper,” “FinOps allocation budgeting AI unit economics cost governance,” “Slurm QOS limits preemption backfill scheduling documentation,” “Codex usage limits weekly allowance,” and “app server account rateLimits read,” followed by targeted paper and documentation searches.

- **FinOps Foundation**, financial accountability and outcome measurement: [Unit Economics](https://www.finops.org/framework/capabilities/unit-economics/) and [Allocation](https://framework.finops.org/framework/capabilities/allocation/).
- **Slurm**, batch scheduler: [Scheduling Configuration](https://slurm.schedmd.com/sched_config.html). Lower-priority work must preserve higher-priority scheduling commitments.
- **Google Cloud Billing**, budget-control example: [budgets](https://docs.cloud.google.com/billing/docs/how-to/budgets). Current documentation distinguishes alerts-only budgets from preview spend caps. “Cloud budgets never enforce caps” would be an outdated generalization.
- **A Unified Approach to Routing and Cascading for LLMs**, theoretical and empirical routing study: [paper](https://arxiv.org/html/2410.10347v3). Its SWE-Bench experiment assumes reference-test verification and uses latency-based costs.
- **SWE-Router**, multi-turn coding-routing preprint: [paper](https://arxiv.org/html/2607.00053v1). Reports favorable cost-resolution curves from partial-trajectory routing. Its mixed training setup includes benchmark data; the specified held-out slice is smaller than the full benchmark.
- **Scrouting / SuperScout**, repository-preparation and routing preprint: [paper](https://arxiv.org/html/2608.04804v1). The no-router handoff baseline ties the routed system. Results cover one benchmark’s Python slice across three repositories; generality is unverified.
- **EvalPlus**, code-verification research: [paper](https://arxiv.org/abs/2305.01210). Stronger test suites exposed previously undetected incorrect outputs.
- **T2MO**, proposed enterprise routing methodology: [paper](https://arxiv.org/html/2608.08528v1). Useful design hypotheses; explicitly positioned for future empirical study.
- **OpenAI Codex**, coding host: [usage documentation](https://learn.chatgpt.com/docs/pricing#what-are-the-usage-limits-for-my-plan) and [app-server interface](https://learn.chatgpt.com/docs/app-server#api-overview-1). Documents shared allowances, possible weekly limits, rate-limit reads, and account usage summaries. These capabilities were not tested locally.
- **Claude Code**, coding host and telemetry example: [status-line contract](https://code.claude.com/docs/en/statusline). Documents allowance-window observations, optional fields, estimated list-price cost, and context counters that must not be confused with cumulative spend.
- **CoDyn**, coding-routing paper candidate: its [OpenReview PDF](https://openreview.net/pdf?id=0ox03jE6jb) returned a browser challenge. No numerical findings from its search snippet were used.