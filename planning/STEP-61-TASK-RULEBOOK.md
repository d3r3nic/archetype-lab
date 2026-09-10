# Step 61: Shared task and freshness rulebook

Trigger: onboarding an existing planning repository exposed a missing global contract for task ownership, evidence, platform access, and freshness. The repository had local agent guidance; injection did not install a shared AGENTS.md entry point. General guidance belongs in Archetype; private project mappings do not.

Decision: keep the existing factory → framework product → project installation flow. Add routed protocols, not another convention catalog or independent rule source. This is an extension of context management, verification, steering, and documentation.

| Layer | Deliverable |
| --- | --- |
| Framework | bootstrap/REPOSITORIES.md: repository ownership, task branches, safe adoption, source reuse |
| Framework | development/TASKS.md: one canonical task source, bounded execution, evidence and completion |
| Framework | development/FRESHNESS.md: source versions, invalidation, action gates, recovery and honest limits |
| Framework | templates/task-context.md: project-owned task and platform binding |
| Entry points | Managed AGENTS.md routes to the shared enforcer; installer preserves existing guidance; updater refuses unmanaged entry-point replacement |
| Project | References.md, feature-tree.md, local additions, task binding, and explicit runtime capability status |

The framework defines behavior. Runtime enforcement, provider APIs, credentials, current prices, organization mappings, and application-specific state names belong to the consuming project. A project without a running task service uses a declared local planning source. A connected project cannot silently create an offline second task authority during an outage.

Freshness promise: stale, invalidated, or unverifiable prerequisites cannot be silently accepted as current. Source checks carry revisions and observation times. Internal updates require version-checked writes; external changes between observation and action cannot be excluded unless that source supports an atomic precondition. Never promise absolute zero staleness across independent services.

Plan: publish this factory decision first; implement and verify the generic protocols and entry-point distribution; independently review before merging product changes; onboard the existing project using the released framework. Preserve all original local guidance and docs. Automated fleet rollout and runtime action enforcement are separate implementation work, not claimed by a documentation release.

Acceptance: source self-test; installed self-test; existing entry-point backup; repeat injection refuses before mutation; update propagates managed guidance while preserving local files; unmanaged entry-point update fails safely. Review the protocol against duplicate tasks, canceled work, stale authorization, expired observations, uncertain external results, and unavailable platform access.

Status: planned; implementation and validation evidence will be recorded in the factory changelog. No private project names, repository mappings, or credentials belong in this public record.
