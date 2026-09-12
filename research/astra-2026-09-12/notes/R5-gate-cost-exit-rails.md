## Decision this informs

Adopt all four additions as project obligations: prove merge enforcement, track operating and development costs, record service exit paths, and define owner authorization boundaries. Extend existing conventions and protocols rather than adding another numbered convention.

The brief needs these corrections:

- The backend scaffold **already requires failing typecheck and test changes to demonstrate blocked merges**. The gap is retained evidence, refresh rules, and coverage across project shapes. [SCAFFOLD-BACKEND.md:249](/Users/d3r3nic/Development2/ai-dev-framework/dist/scaffolding/SCAFFOLD-BACKEND.md:249)
- The platform template already records projected cost fit, export policy, and instructions for leaving. It lacks a recurring ledger and service-specific migration assessment. [references-platform.md:31](/Users/d3r3nic/Development2/ai-dev-framework/dist/templates/references-platform.md:31), [references-platform.md:173](/Users/d3r3nic/Development2/ai-dev-framework/dist/templates/references-platform.md:173)
- Budget and authorization freshness already appear in the task protocols. Reuse that contract. [TASKS.md:15](/Users/d3r3nic/Development2/ai-dev-framework/dist/development/TASKS.md:15), [FRESHNESS.md:11](/Users/d3r3nic/Development2/ai-dev-framework/dist/development/FRESHNESS.md:11)
- The local inventories contained no pipeline definitions or license files, consistent with the recorded follow-ups. This does not establish whether externally configured CI exists. [STEP-63-SELF-HONEST-PLAYBOOKS.md:102](/Users/d3r3nic/Development2/ai-dev-framework/planning/STEP-63-SELF-HONEST-PLAYBOOKS.md:102)

## Durable principles

1. **Prove a control rejects the prohibited action.** Configuration, execution, and enforcement are different facts. This prevents a green pipeline being mistaken for a protected merge path. Evidence: code-host documentation distinguishes required checks, bypass permissions, and direct-push access. The proposed evidence bundle below is my design synthesis. [Branch protection](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches), [Push and merge permissions](https://docs.gitlab.com/user/project/repository/branches/protected/)

2. **Show costs before optimizing them.** Separate estimates, invoices, allocations, and unknowns. This prevents surprise bills and invented per-feature precision. Evidence: industry practice connects allocation with reporting, budgets, forecasts, and explicit shared-cost methods. [Allocation](https://www.finops.org/framework/capabilities/allocation/), [Budgeting](https://www.finops.org/framework/capabilities/budgeting/)

3. **Know the exit without building it prematurely.** Assess exportability, replacement effort, continuity, and commercial constraints. This prevents both accidental dependence and expensive portability work without business value. Evidence: public-sector guidance explicitly balances managed-service benefits against switching costs. [Technical lock-in guidance](https://www.gov.uk/guidance/managing-technical-lock-in-in-the-cloud)

4. **Keep execution authority outside the agent’s self-assessment.** Instructions guide behavior; scoped credentials and execution controls constrain it. This prevents an agent approving its own destructive or externally consequential action. Evidence: security guidance recommends independent authorization validation and approval bound to the specific action. [Agent security guidance](https://cheatsheetseries.owasp.org/cheatsheets/AI_Agent_Security_Cheat_Sheet.html)

## Proposed contract for Archetype

Everything below is **proposed**, not existing enforcement.

Use `References.md` for current policy, `VERSION-LOG.md` for decisions and evidence references, and the existing project task binding for runtime capabilities. That binding already has permission, budget, evidence, and recovery fields. [task-context.md:16](/Users/d3r3nic/Development2/ai-dev-framework/dist/templates/task-context.md:16)

Justify two additional project artifacts:

- `docs/evidence/merge-gate/`: structured manifests and retained, sanitized evidence, because historical proof should survive transient run-log retention.
- `COST-LEDGER.md`: schema-defined monthly and feature-cost tables, because accumulating financial history would overwhelm `References.md`.

Keep confidential evidence in an access-controlled store; project documents may retain stable references and integrity hashes.

**A. Proof of gate**

Proposed wording:

> Before relying on a merge gate, retain evidence identifying the protected destination, permitted actors, effective policy, required checks, and an observed rejection caused by a failing required check. A successful check run alone is insufficient.

The minimum bundle contains:

| Evidence | Required content |
|---|---|
| Effective policy | Repository and target references, observation time, required check identities and trusted producers, push permissions, bypass actors, policy/configuration revision |
| Negative probe | Candidate revision, actor, deliberately failing check, run identifier, host rejection and reason, confirmation that the target remained unchanged |
| Positive control | Equivalent passing candidate reaches merge eligibility, demonstrating the negative probe was not merely rejected for missing permissions |
| Review | Independent audit receipt bound to the reviewed revision, with reviewer/workflow provenance |
| Retention | Evidence locations, hashes, coverage limitations, next review and invalidation conditions |

Test safely on a path that cannot affect customers. A replica proves its own behavior; claiming coverage of another destination requires verified policy and identity equivalence. Otherwise label coverage partial.

Before each merge, require applicable checks and independent review for the actual candidate and integration context. Missing, cancelled, failed, or unaccountably skipped checks must not become a pass. Prevent ordinary agent credentials from bypassing the gate or directly updating protected destinations.

Refresh the rejection proof after changes to policy, workflow triggers, check names/producers, bypass permissions, merge method, or host capability. Also repeat on a project-defined schedule. Revalidate effective protection before consequential merges, subject to the recorded freshness policy.

**Mechanical:** validate manifest fields, artifact integrity, revision bindings, required-check coverage, expiry, and observed rejection reason. A host adapter can compare effective protection with the recorded policy.

**Judgment:** determine whether the probe covers realistic bypasses, whether checks are meaningful, and whether review was actually independent. A hash proves integrity relative to the manifest, not truth.

For projects without a merge path, record “not applicable,” why, and the trigger that activates this requirement. An unsupported protection feature is a capability gap, not evidence of enforcement.

**B. Running-cost ledger**

Proposed wording:

> At bootstrap, record expected operating costs and the owner’s authorized spending limits. Reconcile actual costs each billing period and attribute AI development costs to features where evidence supports attribution. Missing measurements remain unknown.

For each contemplated profile, record:

- Profile identifier, currency, normal and high-usage monthly estimates, usage assumptions, pricing sources and verification dates.
- Recurring services, minimum commitments, variable charges, backups, monitoring, support, transfer costs, and treatment of taxes and credits.
- Owner-authorized operating ceiling, development-AI allowance, warning thresholds, and actions allowed when approaching them.

A profile does not grant spending authority. Record any profile ceiling separately from the owner’s authorized limit.

Monthly ledger rows contain period, service/account identifier, environment, estimate, actual-to-date or finalized actual, forecast, variance, source observation time, and allocation method. Preserve adjustments rather than rewriting past invoices.

Feature rows contain feature/task identifier, sessions or usage records, directly billed AI cost, allocated subscription share, measurement basis, and completeness. Separate development AI from AI consumed by the deployed product.

Use authorized billing exports, invoices, usage interfaces, or host-provided records. Subscription allocation is an accounting estimate, not incremental cash spend. If feature usage is unavailable, record an unallocated shared amount. Never price guessed token counts as measured expenditure. Industry guidance recognizes uncertainty in AI cost data and the need to adapt allocation and budgeting. [AI cost practice](https://www.finops.org/framework/technology-categories/ai/)

Warn when forecast exceeds the approved ceiling, an anomaly crosses the project threshold, credits approach expiry, or stale billing data prevents a trustworthy forecast:

> “At this pace, this month may cost [amount], above your [limit]. I recommend [specific action].”

Pause new discretionary spending when authorization is insufficient. Do not shut down live customer service merely because an alert fired. A budget alert is not a hard spending cap.

**Mechanical:** check required periods, currencies, arithmetic, duplicate imports, allocation reconciliation, freshness, thresholds, and feature references.

**Judgment:** assess forecasts, allocation fairness, and whether savings justify their effect on the product.

**C. Exit paths**

Proposed wording:

> Before adopting a managed service, record how the project could leave it, what cannot move directly, and why its benefits justify the dependency.

Use one `References.md` row per adopted service, identified by category:

`Service/category | data and configuration export | replacement/rewrite | exit time/cost | lock-in rating and reason | evidence date/review trigger`

A provider offering several categories may need several rows. “No retained data” is valid only with a reason; configuration and operational dependencies still matter.

Record export format, access rights, completeness limits, destination/import requirements, identity migration effects, egress or termination charges, and any unavailable proprietary behavior.

Use an explicit rubric:

- **Low:** usable export and limited replacement work.
- **Medium:** supported export but material integration or operational changes.
- **High:** incomplete export, substantial proprietary behavior, or migration beyond accepted cost/outage limits.
- **Unknown:** insufficient evidence, never silently rated low.

Review before adoption, renewal, major schema/integration changes, material price or terms changes, criticality changes, and the project’s maintenance interval.

For critical dependencies, require an executable transition plan and proportionate export/import or recovery exercises. A one-line record is not sufficient regulatory evidence: applicable financial-sector rules require comprehensive, documented, sufficiently tested and periodically reviewed exit plans for services supporting critical or important functions. [Article 28(8)](https://eur-lex.europa.eu/eli/reg/2022/2554/oj)

**Mechanical:** compare the service inventory with exit rows; require fields, valid ratings, dates, and test references where policy requires them.

**Judgment:** assess export completeness, replacement feasibility, acceptable interruption, and regulatory applicability.

**D. Rider safety rails**

Owner-facing wording:

> I handle technical decisions within your agreed scope. These actions require your authorization, either for the specific action or through a clearly bounded standing instruction:
>
> - Start a paid service, accept a recurring commitment, or spend beyond your approved allowance.
> - Send anything to customers, publish externally, sign terms, or make promises on your behalf.
> - Change a live customer system, including pushing or merging into a branch that automatically deploys.
> - Permanently erase valuable data or work, or change live access and secrets in ways that could lock people out.
> - Change what the product does, who it serves, or the promises it makes.
>
> I never expose secrets in code, messages, or logs; invent evidence; bypass required independent review; or weaken a required gate to make failing work appear complete.

These derive from the founder’s escalation categories and quality requirements. [Engineering constitution:3](/Users/d3r3nic/.claude/CLAUDE.md:3)

Routine reversible engineering within the authorized task proceeds without another permission request. Local credential maintenance differs from rotating a live credential. Approved customer workflows differ from unsolicited messages.

Before a restricted action, prepare the concrete change and effect summary. Bind authorization to the action, target, parameters/content revision, spending limit, and applicable duration. Preserve valid standing authorization; changed scope requires reassessment. Silence does not create new authorization.

The execution boundary should validate this authority independently. Use scoped credentials, protected deployment paths, and approval-controlled operations. Where those controls are unavailable, declare the limitation and withhold the relevant privileged capability.

The current destructive-command hook inspects a particular shell-tool event and matches command patterns; the post-task hook is advisory. Neither establishes this broader authorization contract. [pre-destructive-warn.sh:23](/Users/d3r3nic/Development2/ai-dev-framework/dist/bootstrap/hooks/pre-destructive-warn.sh:23), [post-task-verify.sh:3](/Users/d3r3nic/Development2/ai-dev-framework/dist/bootstrap/hooks/post-task-verify.sh:3)

**Mechanical:** test rejection of absent, expired, revoked, mismatched, and replayed approvals; enforce resource and budget limits; protect policy changes; retain receipts. Command matching and secret scanning provide partial coverage.

**Judgment:** determine customer impact, product-scope changes, and whether a standing instruction covers the proposed effect.

**License decision**

A permissive license best fits installation into private customer projects. File-level copyleft fits a deliberate policy of sharing changes to framework files while keeping separate customer files private. Stronger copyleft needs careful distribution and template-copying analysis; using a development tool does not automatically license all its output. [Permissive grant](https://opensource.org/license/mit), [File-level reciprocity](https://www.mozilla.org/en-US/MPL/2.0/FAQ/), [Output and private-use provisions](https://opensource.org/license/gpl-3.0)

The founder decides commercial reuse rights, whether downstream modifications must be shared, and which rights-holding entity licenses the work. Inventory third-party material before licensing it, and preserve notices through installation and updates. Public visibility alone does not supply a general reuse license. [Absence of a license](https://choosealicense.com/no-permission/)

## What the interview must ask the user, in plain words

Ask only unanswered business questions:

- “What monthly running cost is comfortable, and what amount must I never commit you to without asking?”
- “How much may I spend building this, including AI usage?”
- “What data could you never afford to lose, and how long could the service be unavailable?”
- “Do customers or contracts require you to move their data elsewhere?”
- “Which repeat actions have you already authorized, and within what limits?”

The AI chooses checks, evidence formats, service boundaries, warning cadence, and migration techniques using the answers, current primary documentation, and observed capabilities. Present a recommendation when a business tradeoff remains.

## Risks and open questions

- Existing steering prose still assigns architecture decisions and broad implementation approvals to the human. Reconcile it with the founder’s narrower escalation policy instead of stacking contradictory instructions. [19-steering.md:17](/Users/d3r3nic/Development2/ai-dev-framework/dist/conventions/19-steering.md:17)
- Historical gate evidence cannot guarantee current protection. Administrative changes, billing delays, and incomplete exports remain explicit limitations.
- I did not inspect live repository settings, billing accounts, downstream audit evidence, or actual export capabilities. No gate probes, writes, or commits were performed.
- The founder still needs to approve the license grant. Legal applicability for a particular customer requires project-specific assessment.

## Tiny version

**One to two days, an engineering estimate assuming existing access:**

1. Wire the existing checks into both repositories’ actual merge controls and retain a safe failing-change probe plus protection evidence.
2. Add cost and exit tables to bootstrap outputs, with manual invoice reconciliation initially.
3. Add the owner-facing rails and capability declarations to existing routing and task bindings.
4. Implement a proposed `validate-project-controls` checker for schemas, references, freshness, and arithmetic; prove its failures with fixtures and independently audit it.

The full design adds authenticated host observations, automated billing reconciliation, parameter-bound execution approvals, and critical-service exit exercises. The tiny version must label these capabilities unimplemented where absent.

## Research Notes

Dated notes: anything named in this section is an example from the time of writing and expires.

All sources below were read on **2026-09-12**. Searches covered protected-branch bypasses and required checks; cost allocation, showback, budgeting and AI costs; financial-sector exit strategies; public-sector cloud lock-in; agent authorization; and license categories. Recommendations above are my synthesis unless explicitly attributed.

| Source or example | Durable category and finding |
|---|---|
| [GitHub protected branches](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches) | Code-host enforcement. Admin bypass and accepted check conclusions require inspection. |
| [GitLab protected branches](https://docs.gitlab.com/user/project/repository/branches/protected/) | Code-host enforcement. Effective push/merge permissions and overlapping rules matter. |
| [FinOps allocation](https://www.finops.org/framework/capabilities/allocation/), [budgeting](https://www.finops.org/framework/capabilities/budgeting/), [AI practice](https://www.finops.org/framework/technology-categories/ai/) | Cost governance. Explicit allocation, forecasts, actuals, and uncertainty. |
| [UK government lock-in guidance](https://www.gov.uk/guidance/managing-technical-lock-in-in-the-cloud) | Public-sector practice, not a universal legal mandate. Balance value against exit effort. |
| [EU DORA, Article 28(8)](https://eur-lex.europa.eu/eli/reg/2022/2554/oj) | Financial-sector regulation. Tested exit planning for applicable critical or important dependencies. |
| [OWASP agent security](https://cheatsheetseries.owasp.org/cheatsheets/AI_Agent_Security_Cheat_Sheet.html) | Security practice. Independent action validation and bounded approvals. |
| [MIT](https://opensource.org/license/mit), [Apache-2.0](https://www.apache.org/licenses/LICENSE-2.0), [MPL-2.0 FAQ](https://www.mozilla.org/en-US/MPL/2.0/FAQ/), [GPL-3.0](https://opensource.org/license/gpl-3.0) | Permissive and copyleft categories. Terms differ on notices, patents, and redistribution. |

**Founder recommendation:** approve **Apache-2.0** for owned framework code, templates, and documentation, because it permits private commercial use while providing an explicit contributor patent grant and preserving attribution obligations. [License terms](https://www.apache.org/licenses/LICENSE-2.0)

The GNU license FAQ and two additional code-host pages failed to fetch; no conclusions rely on those failed requests.