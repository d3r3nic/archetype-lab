# Step 62: Timeless conventions, enforced

Trigger: bootstrapping a downstream text game against product revision 326a030 surfaced that the published conventions still name tools and vendors as the answer, carry factory changelog language, and state era-bound numbers as law. Factory rule 5b ("framework encodes character; project artifacts hold specifics") existed but nothing enforced it. An audit on 2026-09-11 using a fixed grep pattern (recorded below; reproduced in the factory session: 65 hits across 14 of 29 conventions) plus a read of CLAUDE.md and Conventions.md established the trigger, and the file templates listed dated library placeholders.

Trigger pattern, run with `grep -oE` per file inside dist/ at 326a030, hits counted per file:

```
MUI|Chakra|Radix|Ant Design|FluentUI|Tailwind|Next\.js|NEXT_PUBLIC|EXPO_PUBLIC|Expo|React|Vue|Svelte|Redux|Zustand|Jotai|React Query|SWR|Apollo|Zod|Yup|Valibot|Pydantic|Prisma|Auth0|Cognito|Supabase|Vercel|AWS|Figma|Claude Design|ESLint|Ruff|Flake8|golangci|Clippy|Roslyn|RuboCop|PHPStan|husky|lint-staged|lefthook|helmet|django|Snyk|Socket\.dev|pip-audit|axios|Storybook|Jinja|Vite|Webpack|Turbopack|Playwright|Cypress|Jest|Vitest|Bandit|Step 4[0-9]|Step 6[0-9]|2\.74x|45%|80-90%|300 lines|25-30 minutes
```

The pattern is unbounded, so it also matches ordinary words; after remediation its one residual hit outside Research Notes is "Expo" inside "Exposing" in #26.

Decision: the rule becomes a gate. Conventions describe WHAT and WHY; tools and vendors are chosen per project at bootstrap and recorded in References.md. A new validator runs inside the framework self-test and fails the check on expirable content. The remediation rewrites form, not intent: every rule keeps its direction, and where a number or a taste was doing the work of a principle, the principle is stated and the number becomes a project-set dial.

## The rule

Outside a section titled exactly "Research Notes", a convention, enforcer, index, README, or file template names no product, library, service, or vendor. It cites no factory step. It attaches no statistic or multiplier to a claim about AI. It states no line-count limit or minute cadence as a fixed number. It carries no changelog language (version-scoped headers, "shipped", "not yet implemented", "added in"). Every Research Notes section opens with a line beginning "Dated notes:" so a reader knows anything named there expires.

## Three staleness classes

1. Tool and vendor names inside the rule itself. Examples removed: "installing MUI (or equivalent)", "Tailwind defaults", "@auth0/auth0-react", "Cognito sub", "helmet for Node", "npm audit, Snyk, Socket.dev, pip-audit", the linter and hook vendor list in #25, "Figma / Claude Design / tokens.json", NEXT_PUBLIC_SITE_CONFIG and getSiteConfig() in CLAUDE.md, the Prisma getDb example, babel-preset-expo, the `--t-*` and oklch CSS in #6, and every bracketed library list in the References templates.
2. Factory leakage into product content. Examples removed: "Per framework Step 49" (#6), "Step 48 added this" and "v2 shipped" (#26), "Step 49 `Type: template`" (#27), pulse monitor implementation details in the convention (snapshot file name, inspector script, bundler and symlink gotchas), version-scoped headers in the pulse spec template.
3. Era-bound beliefs stated as law. Examples rewritten as principles: "keep files under 300 lines" (#1, #17) and "beyond 200 lines" (#4) become "small enough for the tool in use to read whole; the project sets its limit in References.md"; "every 25-30 minutes" (#17) becomes "before the tool's window fills"; "AI does this at 80-90%" (#3, #19), "2.74x more vulnerabilities, 45%" (#23), "over-comments 90-100%" and "30-40% fewer tokens" (#16), "~80% compliance" (hooks spec) become the qualitative claim they were standing in for; "dark mode is mandatory" (#6) becomes "more than one scheme from day one, light and dark the default pair, the project records its scheme set"; "never build standard UI components from scratch" (#22) becomes "prefer an established foundation when one fits the product; a project may rule otherwise and records it in References.md"; "under 5 seconds" for hooks (#25) becomes a project-set budget.

## Allowed zone

A section titled exactly "Research Notes" may name tools and must open with the notice: `Dated notes: anything named in this section is an example from the time of writing and expires. Verify current options at bootstrap.` Nothing outside it may name a tool. The remediation still removed most names from Research Notes in favor of categories; the zone exists so a future author has one lawful place for a clarifying example, not so names accumulate.

## Allowlist policy

`scripts/timeless-allowlist.txt` holds per-file exceptions as `path<TAB>term<TAB>justification`. Every entry needs a one-line justification or the validator fails. Every entry must match something or the validator fails; a placeholder entry is a violation. The list holds one entry: `Claude Code` in `templates/hooks-spec.md`, because the two shipped hook configs are written in that host's settings format and the section naming it is marked dated.

Terms live in `scripts/timeless-terms.txt` as case-sensitive whole-word extended regexes with a category, longest matched first so multi-word names win. Languages, open standards (HTTP, JSON, OWASP, WCAG, ARIA, OpenAPI, GraphQL), regulations, and the platforms a product ships to (iOS, Android, the web) are not terms: they are not tool choices. Common English words that double as product names (Black, Square, Monday, Provider, Bootstrap) and product names that are ordinary capitalized words at sentence start (Express, Relay, Remix, Cursor, Notion) are deliberately absent and left to review. Prefix and extension terms carry their own tail (`NEXT_PUBLIC[A-Z_]*`, `[A-Za-z0-9_-]*\.tsx`) because the word boundary treats underscores and letters as word characters. The list only grows.

| Layer | Deliverable |
| --- | --- |
| Validator | scripts/validate-timeless.sh: classes A-F below, allowlist hygiene, default scope, bash + awk + grep only |
| Validator data | scripts/timeless-terms.txt (category-tagged term list), scripts/timeless-allowlist.txt (justified exceptions) |
| Self-test | scripts/validate-framework.sh group 10 delegates to the validator; the framework check fails on any finding |
| Conventions | 29 universal and 7 backend conventions remediated; #26 and #28 gained an "Applies when" section; #8 gained the transpiled-error rule that CLAUDE.md used to carry as a snippet |
| Enforcers and index | CLAUDE.md, Conventions.md, backend/Conventions.md (count corrected to 29), README.md |
| Templates | references-frontend/backend/mobile/platform placeholders name categories; pulse-monitor-spec carries the implementation notes moved out of #26; hooks-spec host contract marked dated; convention-template carries the notice and an optional "Applies when" slot |
| Inspector | scripts/pulse-inspect.sh header points at the spec as the only contract home (comment change only) |

## Validator classes and scope

A: named tool outside Research Notes (term list, allowlist). B: factory step reference, defined as `Step` followed by two or more digits, or `framework Step N` / `factory Step N`, or `Step N added|shipped|introduced|landed|promoted`; playbook step references are single-digit inside a named document and stay legal. C: a percentage or `Nx` multiplier on a line that also mentions AI, agents, models, LLMs, compliance, or generated code (trigger words match in either case). D: a line-count limit (`under|beyond|over|at|to|around|... N lines`, `N+ lines`, `N-line limit|file|component|target`) or a minute cadence (`every|within|... N minutes`, `N-minute cadence|interval|compaction|session`), with the limit words matching in either case; the pattern leans toward false positives, which cost a rewording, rather than false negatives; a project-set dial names no number and never matches. E: version-scoped headers, `(shipped)`, `not yet implemented`, `added in Step|vN`, `promoted from`, and bare `v1`/`v2` scope language; lines inside code fences and JSON-quoted lines are exempt from the bare-version check. F: every Research Notes section carries the dated notice. The unused-allowlist check runs only against the default set, since a subset scan cannot see every file.

Default scope: conventions/, backend/conventions/, root and backend CLAUDE.md and Conventions.md, README.md, templates/*.md. Backend conventions and README were added to the brief's scope because they are conventions and storefront respectively and the same rule applies; the additions accounted for 31 findings (24 names, 7 notices) and 7 findings. Phase playbooks are out of scope for this step; measured with the final validator on 2026-09-11 after remediation of the in-scope set: bootstrap/ 73 findings, scaffolding/ 101, development/ 47, bootstrap/hooks/README.md plus META-BATTLE-TESTING.md 13. Recorded as follow-up below.

Detection is by list and pattern, so a name the list lacks passes; review adds it. Illustrative numbers that are not limits (a 3-second SLA in a comment example, presigned URL expiry ranges in B6, a cache TTL in B7) pass by design; the class is tool-capability limits, not every number. Line counts attached to files or components are treated as limits even in examples, so the illustrative god-component sizes were reworded.

## Remediation record

Findings before remediation, per file, as classes A/B/C/D/E/F. Every file lists 0 after remediation; the clean run is recorded under Proof.

| File | A | B | C | D | E | F | What changed |
| --- | --- | --- | --- | --- | --- | --- | --- |
| CLAUDE.md | 3 | 0 | 0 | 0 | 0 | 0 | #28 bullet without env-var and getter names; database-client example without the library; component rule reworded to "the foundation"; transpiled-error bullet compressed to the signal, fix routed to References.md and #8 |
| Conventions.md | 2 | 0 | 0 | 0 | 0 | 0 | #28 line without env-var and file extension; #13 without the metric brand; #26 and #28 lines say when they apply |
| README.md | 7 | 0 | 0 | 0 | 0 | 0 | vendor examples in the experience-level paragraph, tree listing, and Key Principles replaced by categories; convention range corrected to 01-28; verification line names the timeless check |
| backend/Conventions.md | 0 | 0 | 0 | 0 | 0 | 0 | universal count corrected 28 to 29 (not a validator finding) |
| backend/conventions/B1 | 0 | 0 | 0 | 0 | 0 | 1 | notice |
| backend/conventions/B2 | 2 | 0 | 0 | 0 | 0 | 1 | pagination and batched-loading described by pattern, not library; notice |
| backend/conventions/B3 | 0 | 0 | 0 | 0 | 0 | 1 | Research Notes middleware example generalized; notice |
| backend/conventions/B4 | 4 | 0 | 0 | 0 | 0 | 1 | logging library list replaced by category; notice |
| backend/conventions/B5 | 2 | 0 | 0 | 0 | 0 | 1 | chat-vendor name in examples generalized; notice |
| backend/conventions/B6 | 12 | 0 | 0 | 0 | 0 | 1 | object-storage vendors and SDKs generalized throughout; magic-byte library examples removed; notice |
| backend/conventions/B7 | 3 | 0 | 0 | 0 | 0 | 1 | cache providers generalized; client-library list removed; notice |
| conventions/00 | 0 | 0 | 0 | 0 | 0 | 1 | notice |
| conventions/01 | 0 | 0 | 0 | 2 | 0 | 1 | file-size limit is a project dial; violation reworded; notice |
| conventions/02 | 0 | 0 | 0 | 1 | 0 | 1 | commit example without a line-count; notice |
| conventions/03 | 0 | 0 | 1 | 0 | 0 | 1 | phantom-requirement rate dropped; notice |
| conventions/04 | 0 | 0 | 0 | 3 | 0 | 1 | component-size limit is a distinct project dial; violation and illustrative size reworded; notice |
| conventions/05 | 0 | 0 | 0 | 0 | 0 | 1 | notice |
| conventions/06 | 1 | 1 | 0 | 0 | 0 | 1 | scheme rule stated as principle with project-recorded scheme set; utility-framework example generalized; template-vs-product section without Step 49, CSS names, or code; Research Notes aligned with #22; notice |
| conventions/07 | 0 | 0 | 0 | 0 | 0 | 1 | notice |
| conventions/08 | 0 | 0 | 0 | 0 | 0 | 1 | new rule: custom errors must survive the build target (moved from CLAUDE.md); notice |
| conventions/09 | 2 | 0 | 0 | 0 | 0 | 1 | batched loading and HTTP client named by category; notice |
| conventions/10 | 0 | 0 | 0 | 0 | 0 | 1 | notice |
| conventions/11 | 5 | 0 | 0 | 0 | 0 | 1 | provider names in violations and examples generalized; notice |
| conventions/12 | 0 | 0 | 0 | 0 | 0 | 1 | notice |
| conventions/13 | 0 | 0 | 0 | 0 | 0 | 1 | notice |
| conventions/14 | 0 | 0 | 0 | 0 | 0 | 1 | notice |
| conventions/15 | 7 | 0 | 0 | 0 | 0 | 1 | linter list in RIGHT example dropped; console-output wording; notice |
| conventions/16 | 4 | 0 | 2 | 0 | 0 | 1 | over-commenting and token-saving stats dropped; ticket and doc-comment examples generalized; notice |
| conventions/17 | 0 | 0 | 0 | 3 | 0 | 1 | file-size limit and compaction cadence are project dials; notice |
| conventions/18 | 0 | 0 | 0 | 0 | 0 | 1 | notice |
| conventions/19 | 0 | 0 | 1 | 0 | 0 | 1 | scope-creep rate dropped; notice |
| conventions/20 | 0 | 0 | 0 | 0 | 0 | 1 | notice |
| conventions/21 | 0 | 0 | 0 | 0 | 0 | 1 | notice |
| conventions/22 | 10 | 0 | 0 | 0 | 0 | 1 | rewritten: prefer an established foundation when one fits; project may rule otherwise in References.md; wrapper, token, and catalog rules unchanged; notice |
| conventions/23 | 0 | 0 | 1 | 0 | 0 | 1 | AI vulnerability statistics replaced by the qualitative claim; Research Notes middleware and scanner names replaced by categories; notice |
| conventions/24 | 0 | 0 | 0 | 0 | 0 | 1 | Research Notes database examples generalized; notice |
| conventions/25 | 8 | 0 | 0 | 0 | 0 | 1 | formatter, suppression-directive, HTTP-client, and hook-vendor names generalized; hook speed is a project dial; Research Notes list by category; notice |
| conventions/26 | 6 | 1 | 0 | 0 | 7 | 1 | rewritten: "Applies when" section; version headers, step reference, implementation gotchas, and future list removed (implementation notes moved to the spec template); framework-specific examples generalized; notice |
| conventions/27 | 2 | 2 | 0 | 0 | 0 | 1 | lifecycle table without Step 49 or design-tool names; notice |
| conventions/28 | 8 | 0 | 0 | 0 | 0 | 1 | "Applies when" section; schema library, env-var, and getter names described as template-local; handoff doc reference generalized; notice |
| templates/convention-template.md | 0 | 0 | 0 | 0 | 0 | 1 | notice, optional "Applies when" slot, and the zone rule stated in the template |
| templates/feature-tree.md | 1 | 0 | 0 | 0 | 0 | 0 | diagram tool name dropped |
| templates/feature-tree-platform.md | 5 | 0 | 0 | 0 | 0 | 0 | platform examples replaced by categories |
| templates/hooks-spec.md | 0 | 0 | 2 | 0 | 0 | 0 | compliance percentages dropped; host contract section marked dated with the host allowlisted and its literal event names kept there; second host name dropped |
| templates/pulse-monitor-spec.md | 7 | 0 | 1 | 0 | 9 | 0 | rewritten without version narrative; contract example matches the inspector's emitted version; implementation notes and drift section added from #26; framework names replaced by categories |
| templates/references-backend.md | 44 | 0 | 0 | 0 | 0 | 0 | tech-stack and system placeholders name categories; slots for the file-size, compaction, pre-commit-budget, and build-target dials |
| templates/references-frontend.md | 33 | 0 | 0 | 0 | 0 | 0 | same; slots for every dial including foundation decision, component size, and color schemes |
| templates/references-mobile.md | 36 | 0 | 0 | 0 | 0 | 0 | same; store fees, versions, and beta-distribution products become verify-at-bootstrap placeholders; managed-vs-bare section generalized; dial slots added |
| templates/references-platform.md | 24 | 0 | 0 | 0 | 0 | 0 | platform, analytics, and payment examples replaced by categories; decision-log examples use placeholders |

Totals before, measured with the final validator on an export of 326a030: 312 findings in 48 files (A 238, B 4, C 8, D 9, E 16, F 37). After: 0. (The first validator revision counted 307; the audit-driven fixes to prefix matching and the limit patterns account for the difference.)

Judgment calls, stated so they can be overruled: the scheme rule in #6 keeps "from day one" as the durable part and makes the scheme set a recorded project decision; the audit read "dark mode is mandatory" as taste, the founder's earlier steering read "dark mode from day one" as a signal, and this wording keeps both. Languages are not vendors and a language example that clarifies a language-agnostic rule stays (CLAUDE.md's untyped-escape-hatch example). Seconds are not validated; the one hook-speed number was rewritten by hand. English-word product names are not terms; review catches them. Illustrative code in #28 keeps a getter name and says it is the example project's choice.

## Proof

Planted-violation run on a copy of the remediated dist, one violation per class (two for A and C, covering the audit's blocker on underscore-bearing names and lowercase trigger words) plus two bad allowlist entries (script kept outside the product; output verbatim):

```
$ bash scripts/validate-timeless.sh   (on a copy with planted violations)
Archetype Timeless-Conventions Check
Root: <copy>
FAIL: allowlist entry lacks a justification: conventions/05-state.md	Redux
FAIL: [A] conventions/05-state.md:16: named tool outside Research Notes [state-data]: Zustand
FAIL: [A] conventions/05-state.md:17: named tool outside Research Notes [framework]: NEXT_PUBLIC_SITE_CONFIG
FAIL: [B] conventions/26-pulse-monitor.md:74: factory step reference: - Step 48 added the monorepo scan.
FAIL: [C] conventions/01-project-setup.md:60: statistic attached to an AI claim: - AI hardcodes values 80-90% of the time.
FAIL: [C] conventions/01-project-setup.md:61: statistic attached to an AI claim: - agents hardcode values 80-90% of the time.
FAIL: [D] conventions/17-context.md:41: line-count limit (make it a project-set dial): - Keep files under 300 lines.
FAIL: [D] conventions/17-context.md:42: minute cadence (make it a project-set dial): - Compact context every 25-30 minutes.
FAIL: [E] conventions/26-pulse-monitor.md:74: changelog language: - Step 48 added the monorepo scan.
FAIL: [E] templates/feature-doc-template.md:32: version-scoped header: ## v2 scope (shipped)
FAIL: [E] templates/feature-doc-template.md:34: changelog language: Drift detection is v2 — not yet implemented.
FAIL: [F] conventions/12-testing.md:52: Research Notes section at line 44 lacks a line starting "Dated notes:"
FAIL: allowlist entry matches nothing (remove it): conventions/05-state.md / Redux
FAIL: allowlist entry matches nothing (remove it): conventions/07-types.md / Zod
===
14 timeless violations
exit code: 1
```

Clean run on the remediated dist: `bash scripts/validate-timeless.sh` prints OK for all six classes and both allowlist checks, then `Pass: timeless check clean`, exit 0. `bash scripts/validate-framework.sh` reports group 10 "OK: timeless check clean" and `Pass: 0 errors, 0 warnings`. `python3 scripts/test-entrypoints.py` with `ARCHETYPE_LEGACY_SOURCE` set to an export of product revision 46ebfa4 (the pre-AGENTS release, as Step 61 used): 14 tests, OK. All three observed on 2026-09-11 in the factory dist checkout before publication.

## Independent audit

An Opus agent that did not write the change audited the four product commits and this document on 2026-09-11 (read-only, experiments on copies). Verdict: FIX-FIRST with 13 findings. Blockers: the word boundary excluded underscores, so `NEXT_PUBLIC_SITE_CONFIG` and `Button.tsx` passed; and six rewritten rules delegated a number to References.md without the templates carrying a slot, and CLAUDE.md claimed a constructor fix "is recorded" there. Should-fix: case-sensitive AI-statistic triggers, code fences not tracked for the changelog check, off-by-one out-of-scope counts (an allowlist artifact of subset scans), one surviving beta-distribution product name in the mobile template, the hooks spec describing its own JSON configs without their literal keys, a dangling quote of the old CLAUDE.md sentence in scaffolding/RED-FLAGS.md, a request to restore vendor names inside Research Notes, and three rules it read as weakened (#6, #22, CLAUDE.md's component bullet). Nits: validator robustness cases and two inaccurate sentences in this document.

Closed in product commit 72f76d4: prefix and extension terms carry their own tail; triggers match either case; fences are tracked; the unused-allowlist check is skipped for explicit file lists; two-field allowlist entries no longer suppress; awk interval support is checked; file lists are arrays; limit patterns cover "at/to/around N lines" and "N-minute cadence"; TestFlight and Play Console are terms, and five ordinary-word names left the list; every dial has a slot in the frontend, backend, and mobile References templates; CLAUDE.md's transpiled-error bullet is an instruction; its component bullet covers a project with no foundation; #6 states the swap capability as mandatory and the shipped scheme set as a recorded decision; #22 keeps building from scratch a violation until a standalone component has been searched for; #4 names a distinct component-size dial; #1 and #17 drop "far past"; hooks-spec keeps the host's literal event names inside its dated section; scaffolding/RED-FLAGS.md quotes the current rule and keeps the concrete fix as a dated example. This document's two inaccurate sentences and its counts were corrected.

Declined: restoring vendor names inside Research Notes. The founder's steering for this framework, kept in the founder's session memory outside both repositories, is that Research Notes name categories of tooling, not tools; the zone stays lawful so a future clarifying example has a home, and the notice marks it dated. The three rules read as weakened were judgment calls made on the founder's instruction (#22 wording was specified in the step brief; dark mode was classed as era-bound taste) and are now worded so the structural obligation stays mandatory and only the recorded choice is the project's.

Re-review of 72f76d4 by the same auditor on 2026-09-11: FIX-FIRST with two items open, both closed in a sixth product commit: the follow-up list here still carried the first-revision out-of-scope counts, and the limit words in class D matched lowercase only, so a sentence-initial "Under 300 lines" passed. It also asked for the trigger grep pattern to be recorded (now above) and noted that the hooks spec overstated which event names the shipped configs use (reworded). Final re-review of b00df61 by the same auditor on 2026-09-11: APPROVE, with the two items closed and the recorded trigger pattern reproduced at 65 hits in 14 of 29 conventions.

## Status

Published and verified. Factory design PR #3 (e00981d) preceded product PR #2; product revision 5c305ef590cc2787b9c3aaa81a2c7b6aff1e0d67, the squash of six branch commits (e47ae4b validator, 460bdd5 conventions, f59b20d enforcer and index, bbe4ed9 templates, 72f76d4 and b00df61 audit fixes). Independent audit: FIX-FIRST, then APPROVE after the fix commits. Observed on the merged product main on 2026-09-11: validate-framework.sh 0 errors, 0 warnings; test-entrypoints.py 14 tests OK with the 46ebfa4 legacy export. Downstream projects are not updated by this step; each pulls the revision through its installed updater, the game last.

## Follow-up proposals (not done in this step)

1. Shape-agnostic core. Bootstrapping a text game marked 8 of 16 foundational systems not applicable (API, contract, auth, authorization, database, file storage, forms, routing), and #28 is specific to a multi-customer template business. Timeless also means shape-agnostic: a universal core plus expirable profiles (web app, mobile app, template business). This step added "Applies when" to #26 and #28 and an optional slot in the convention template; restructuring is a separate step.
2. Phase playbooks and remaining docs. bootstrap/ (73 findings), scaffolding/ (101), development/ (47), bootstrap/hooks/README.md and META-BATTLE-TESTING.md (13) fail the same validator today and are outside its default scope. Playbooks legitimately carry more procedure; the same zone rule can apply with a per-file "Research Notes" or a playbook-specific allowed zone. Extend the default scope when they are remediated.
3. Scripts and UI assets are not scanned. templates/pulse-ui/app.js still carries a version comment; harmless, but the same rule should reach comments once the playbooks are in scope.
