# Step 63: The framework tells the truth about itself, and the playbooks obey the timeless rule

Trigger: two findings from the Step 62 follow-ups and the candidate sweep recorded in research/timeless-candidates-2026-09-11.md. First, the framework's own claims about itself were stale and unchecked: six counts of its parts were wrong on the published product (two convention counts in the bootstrap playbook, two red-flag counts in the scaffold playbooks, one in the develop playbook, one in the meta document), several defenses named validator checks that do not exist, and six engine paths pointed at files that are not shipped. Second, the phase playbooks were outside the timeless gate by design in Step 62; measured with the final Step 63 checks on product 5c305ef they carried 205 timeless findings in 16 files and 12 self-claim findings.

Decision: two more invariants, both checked. The shipped documents tell the truth about the framework (scripts/validate-claims.sh, validate-framework.sh group 11), and the timeless gate covers every shipped markdown file, with one more allowed zone for the procedural playbooks. Factory design first, product second, downstream last.

## The rules added

Self-claims. No shipped document states a count of the framework's parts that the tree does not bear out; a count of a red-flag catalogue is forbidden outright, because it changes with every addition, so the document names the catalogue instead. Every path into the engine resolves to a shipped file, with or without the downstream "archetype/" prefix; a project-owned path is marked as such ("or equivalent", "project-owned", "if present", "optionally", "generated") within forty characters, or rewritten as prose. Every "#N" and "BN" reference resolves to a convention file.

Playbooks under the gate. The default scope of validate-timeless.sh is every shipped markdown file except libraries/: conventions, backend, the enforcers and indexes, README, the meta document, templates, bootstrap (with the hooks README), scaffolding, and development. A line beginning "Dated example:" (bullet form allowed) is a per-line allowed zone for names, so a concrete case stays next to its rule and is marked expirable; the other classes still apply to that line. A step a playbook defines as its own heading, or a step named with its playbook ("SCAFFOLD-BACKEND Step 13"), is a playbook step and legal; explicit factory phrasing ("framework Step 49", "Step 45 added") and bare two-digit steps the file does not define are flagged.

| Layer | Deliverable |
| --- | --- |
| Validator | scripts/validate-claims.sh: count claims (digits and number words), id ranges, engine paths, convention references; bash + awk + grep |
| Validator | scripts/validate-timeless.sh: full default scope, "Dated example:" per-line zone, document-defined and playbook-named steps legal, two-pass awk per file |
| Self-test | scripts/validate-framework.sh group 11 delegates to the claims check; group 10 now covers the playbooks through the wider default scope |
| Playbooks | bootstrap/ (6 files), scaffolding/ (7), development/ (7), META-BATTLE-TESTING.md remediated; REPOSITORIES.md, TASKS.md, FRESHNESS.md, MAINTAIN-RED-FLAGS.md needed no change |
| Enforcers | backend/CLAUDE.md routes to B3 (middleware pipeline) and B7 (cache isolation), which no enforcer line reached before; conventions/25 title carries its number like every sibling |
| Scripts | scripts/validate-scaffold.sh header describes the groups the script actually has (the old header advertised a direct-import check that does not exist) |
| Factory | README.md describes the factory as it is (it listed a dist/References.md that never existed and two of six research files) |

## Method

The factory session wrote both validators, the planted-violation proof, and the enforcer and script fixes. Three Opus agents, one per playbook directory, remediated the playbooks in parallel against the final validators, with disjoint file sets and no commits; two stalled once on large single-file rewrites and were resumed with an instruction to edit section by section. The factory session read every changed line of all three diffs before committing them. Agents did not touch scripts/, conventions/, or templates/.

## Remediation record

Timeless findings before, measured with the final validator on an export of 5c305ef, per file; every file is 0 after. Self-claim findings are listed separately.

| File | Timeless before | What changed |
| --- | --- | --- |
| bootstrap/ONBOARD.md | 64 | stack selection and platform-first research run on categories and properties with an explicit research-at-bootstrap instruction; interview examples name no apps; two stale convention counts gone; the factory step reference gone; 493 to 492 lines |
| bootstrap/LEARNING-PROJECTS.md | 5 | enterprise-infra examples by category; one dated example keeps the local-tier versus managed-tier pattern; 64 to 62 lines |
| bootstrap/RED-FLAGS.md | 4 | infra and identity-provider names by category; the coverage threshold stays as a decision rule |
| bootstrap/hooks/README.md | 9 | scripts described by event kind; the host and its event names on two dated-example lines; the dated event-count claim gone |
| bootstrap/EXISTING-PROJECT.md | 0 | two vendor names the term list lacks and one example path generalized |
| bootstrap/REPOSITORIES.md | 0 | unchanged |
| scaffolding/SCAFFOLD.md | 3 | platform examples by category; red-flag count replaced by the catalogue's name |
| scaffolding/_preamble.md | 3 | stack illustration generalized; red-flag count replaced |
| scaffolding/SCAFFOLD-FRONTEND.md | 10 | library names by category; the one concrete deploy trap is a dated example; the factory step reference gone; 312 to 311 lines |
| scaffolding/SCAFFOLD-BACKEND.md | 16 | queue, cache, metrics, container, and CI names by category; the final-gate list names the checks validate-scaffold.sh really runs |
| scaffolding/SCAFFOLD-MOBILE.md | 17 | framework and toolchain names by category; the pulse snapshot and starter UI move to a project-owned directory instead of inside the engine folder |
| scaffolding/SCAFFOLD-PLATFORM.md | 4 | platform names by category; the missing runbook template becomes the task pattern each sector follows; the factory step reference gone |
| scaffolding/RED-FLAGS.md | 19 | defenses say what validate-scaffold.sh checks and where a defense rests on the step itself; toolchain gotchas stated generically |
| development/DEVELOP.md | 9 | commands by reference to References.md; the validator section separates what fails the run from what only warns; red-flag count replaced; 176 to 175 lines |
| development/MAINTAIN.md | 1 | test command by reference |
| development/RED-FLAGS.md | 8 | defenses describe validate-develop.sh as it is; step cross-references corrected to the steps that exist; one dated example keeps the shared-client class names; changelog phrasing recast |
| development/MAINTAIN-RED-FLAGS.md | 0 | changelog phrasing recast as present-tense rules |
| development/CREATING-FORKABLE-TEMPLATES.md | 29 | the general forkability playbook: per-framework table becomes four stack-agnostic checks plus one dated example; machine-local paths, a private project name, and the factory step reference removed; 162 to 152 lines |
| META-BATTLE-TESTING.md | 4 | the evolution mechanism stays; step numbers, the stale convention count, and the step citations in the example table are gone |

Totals before: 205 timeless findings in 16 files (A 193, B 7, C 2, E 3) and 12 self-claim findings (6 counts, 6 engine paths). After: 0 and 0. Six "Dated example:" lines exist in the playbooks after remediation.

Judgment calls, stated so they can be overruled: the "80%+ coverage" threshold in the platform-first rule stays where it is a project decision rule (how much of the use case a platform must cover before custom code is justified); only the two lines that attached it to a claim about what AI or platforms "often" achieve were reworded. Platforms a product ships to (iOS, Android, their stores), open standards (including OpenTelemetry, which the scaffold validator treats as a standard), and regulations stay by the Step 62 policy. The framework's own clone URL and the executable commands in runnable snippets stay. validate-develop.sh still only warns on a raw throw; the prose now says so instead of claiming a gate, and changing the script's severity is a separate decision with downstream effects. The composition order "ErrorBoundary, QueryClient, Theme, Auth, Router" stays in two places as role names that must match.

## Proof

Planted-violation run on a copy of the remediated tree (script kept outside the product; output verbatim):

```
$ bash scripts/validate-claims.sh conventions/12-testing.md conventions/13-performance.md conventions/14-accessibility.md   (copy with planted violations)
FAIL: conventions/12-testing.md:17: claims "27 conventions" but the tree has 29
FAIL: conventions/12-testing.md:17: counts a red-flag catalogue ("twelve silent-failure patterns"); name the catalogue, never its size
FAIL: conventions/12-testing.md:17: id range "01-27.md" but the highest convention is 28
FAIL: conventions/13-performance.md:18: references scaffolding/SCAFFOLD-EDGE.md, which does not exist in the engine
FAIL: conventions/13-performance.md:18: references scripts/validate-bundles.sh, which does not exist in the engine
FAIL: conventions/14-accessibility.md:18: references convention #31 but conventions/31-*.md does not exist
FAIL: conventions/14-accessibility.md:18: references backend convention B9 but backend/conventions/B9-*.md does not exist
===
7 self-claim violations
exit code: 1

$ bash scripts/validate-timeless.sh conventions/05-state.md   (dated-example line and a named playbook step pass; the factory step fails)
OK: allowlist entries carry justifications
OK: no named tools outside Research Notes
FAIL: [B] conventions/05-state.md:18: factory step reference: - This rule was rewritten in framework Step 62.
OK: no statistics attached to AI claims
OK: no tool-capability numeric limits
OK: no changelog language
OK: every Research Notes section carries the dated notice
NOTE: explicit file list given; unused-allowlist check skipped
===
1 timeless violations
exit code: 1
```

Clean runs on the remediated tree, observed 2026-09-11 in the factory dist checkout: `bash scripts/validate-timeless.sh` (full default scope) prints `Pass: timeless check clean`; `bash scripts/validate-claims.sh` prints `Pass: self-claims check clean` with the derived facts `29 conventions, 7 backend, 4 phases, 2 hooks`; `bash scripts/validate-framework.sh` prints `Pass: 0 errors, 0 warnings` with groups 10 and 11 clean; `python3 scripts/test-entrypoints.py` with `ARCHETYPE_LEGACY_SOURCE` set to an export of 46ebfa4 runs 14 tests, OK.

## Independent audit

An Opus agent that did not write the change audited the product branch and this record on 2026-09-11 (read-only, experiments on copies). Verdict: FIX-FIRST with 12 findings and no lost force in any playbook: every step, verify line, gate, red flag, defense, and interview question survives; three places gained force. Blocker: the scaffold validator's header still listed groups it does not have and numbered the rest wrongly. Should-fix: this record said three dangling engine paths where the run shows six; the Research Notes zone was reported at an inflated line number in the two-pass scan; AGENTS.md was outside the timeless default scope; the red-flag count rule let "the 18 patterns in RED-FLAGS.md" through and misread "Phase 3 silent-failure patterns" as a count; an issue number such as "#41" read as a convention reference. Nits: one wrong mobile step cross-reference, one interview script missing the name-a-current-app instruction its twin received, an empty numbered step in the forkability verify snippet, the develop validator's stale comments, a too-narrow playbook-name list in the step rule, OpenTelemetry unrecorded as a standard, a wrong research-file count here.

Closed in product commits 4b12fa2 and 3431360; the record corrected. Re-review found one item still open (a count preceding an ordinal, "7 Phase 3 silent-failure patterns", slipped through the new ordinal guard) and one new nit (a universal count attributed to the backend tree when both appear on one line); both closed in 3431360. Final re-review: APPROVE, with the before-state counts (205 timeless, 12 self-claim) and both planted proofs reproduced verbatim. Acknowledged, not changed: the step rule now exempts a bare two-digit step whenever a playbook name appears earlier on the line, a narrow false-negative; paths inside code fences are checked by the claims check (unlike bare version tokens in the timeless check), harmless today.

## Status

Published and verified. Factory design PR #7 (d69ab27) preceded product PR #3; product revision 3e670a57118a9127f0e726b1cb6c5cd97a228120, the squash of seven branch commits (validators; development, scaffolding, and bootstrap playbooks; two small fixes; two audit-fix commits). Observed on the merged product main on 2026-09-11: validate-framework.sh 0 errors, 0 warnings with groups 10 and 11 clean; test-entrypoints.py 14 tests OK with the 46ebfa4 legacy export. Downstream installations pull the revision through their installed updater; none were updated by this step.

## Follow-up proposals (not done in this step)

1. Nothing runs the gates automatically. Neither repository has a CI definition; every check runs when a person or agent remembers. A pipeline on the product repository running validate-framework.sh and test-entrypoints.py on every push is the smallest fix, and it is a code-host coupling the framework must treat as a project artifact, not a convention.
2. validate-develop.sh detects shared-client bypass from a fixed, dated pattern list (two client classes). The honest prose now says so; reading the client classes from the project's References.md is the timeless form.
3. validate-maintain.sh uses a BSD-only date flag inside a guard, so on GNU coreutils its staleness loop finds nothing; the gates assume one platform's tools inconsistently.
4. The factory's research/ folder and planning/BACKEND-AUDIT.md still carry the dated content Step 62 removed from the product. They are history and are not scanned; a one-line dated notice at the top of each would stop them being read as current.
5. Recorded choices carry no verification date and deferred compliance answers have no slot (sweep candidates C02, C04, C12); host and forge decoupling with pinned installs (C07, C08, C15); the shape-agnostic core (Step 62 follow-up 1).
6. CREATING-FORKABLE-TEMPLATES.md is referenced by no other shipped document; decide whether the develop playbook should route to it.
7. The product ships no license file.
