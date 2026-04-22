# Step 52 — Customer Onboarding & Deployment Automation

Trigger: Edgar's first manual deploy (2026-04-22) surfaced the full sequence, IAM bindings, ignore-file requirements, env discipline, and ingress/DNS work required per customer. Encoded here so subsequent customers are one-command operations. See `planning/CHANGELOG.md` Step 52 for the narrative of findings.

## Architecture bifurcates by billing mode

The original version of this spec assumed every customer gets their own hosting-platform project. That's correct for ONE of the two billing modes, not both.

| Billing mode | Who pays the vendor bill | Service boundary | Ingress resources | Cost shape |
|---|---|---|---|---|
| **Reseller** | The framework consumer (you). Customer pays you via payment processor; you pay the vendor. | All customer services in a single **platform boundary** (one project/tenant under `internal/`). Per-customer cost tracked via resource labels + billing export. | **Shared** ingress: one LB + one static IP + one cert (multi-domain). Per-customer: backend-service + NEG + URL-map entry. | Flat ingress baseline + per-service runtime; amortizes over customer count. |
| **Direct** | End-customer, with their own payment method on the vendor. | **Per-customer boundary** (one project per customer under `customers/<Name>/`). | Per-customer ingress: own LB + own static IP + own cert. | Each customer pays their own ingress baseline. Clean billing isolation; higher fixed cost per customer. |

The script must branch on billing mode. Choosing reseller or direct is a business/finance decision, not a technical one. Both architectures are valid.

## Input: `customers/<name>.yaml`

```
name: <slug>
subdomain: <slug>
stack: nextjs                  # reserved for future multi-stack
billing:
  mode: reseller               # or "direct"
  master_account: <id>         # used when mode=reseller
  customer_account: null       # populated by customer form when mode=direct
repo:
  github_org: <org>
  name: <repo>
  branches: { main: dev, staging: staging, production: prod }
env:
  NEXT_PUBLIC_WP_GRAPHQL_URL: <url>
  NEXT_PUBLIC_CONTACT_EMAIL: <addr>
  # Optional keys: COMMENT OUT rather than ship blank. Schema rejects empty strings.
features:
  blog: true
  products: false
  auth: false
  monitoring: true
  min_instances: 0
```

## Reseller-mode pipeline

### One-time platform setup (done for makemyweb.site 2026-04-22)
- Folder for internal infra under the org.
- Platform project hosting shared LB resources.
- Billing linked to master payment account.
- APIs enabled: compute, run, container builder, artifact registry, certificate manager, IAM.
- IAM bindings:
  - Org admin → project Owner (not auto-inherited on recent projects).
  - Compute-default service agent → container-builder role (not auto-inherited on recent projects).
- Reserved global static IP for the shared LB.
- Shared URL map with per-customer host rules.
- Shared managed multi-domain cert (up to ~100 domains per cert).
- HTTPS target proxy + forwarding rule on the static IP's port 443.

### Per-customer steps (script runs these, idempotent)
1. Deploy Cloud Run service in the platform project, labeled `customer=<slug>`, wired with customer env vars.
2. Create serverless NEG pointing at the Cloud Run service (same-project requirement).
3. Create backend service referencing the NEG.
4. Add `<subdomain>.<root-domain>` to the shared cert (update in place, or split into a new cert at the per-cert limit).
5. Add host rule in the shared URL map: `<subdomain>.<root-domain>` → backend service.
6. DNS: create an A record at the DNS provider pointing `<subdomain>.<root-domain>` → the shared LB's static IP, with the DNS provider's passthrough flag set (no CDN proxy in front of the managed cert's ACME handshake).
7. Poll cert provisioning until the new domain's status is active.
8. Smoke: request the final URL, assert a 200 and confirm routing.

## Direct-mode pipeline

Same general shape, but every resource lives in a customer-owned project:

1. Create a folder under `customers/<Name>/` at the org.
2. Create a project inside that folder.
3. Link the END CUSTOMER's payment account.
4. Enable APIs on the new project.
5. Apply the two-binding IAM fixup (same as above; every new project needs it).
6. Deploy Cloud Run service.
7. Reserve a static IP IN THAT PROJECT.
8. Build the per-customer LB: NEG → backend service → URL map → managed cert → HTTPS proxy → forwarding rule, all scoped to this project.
9. DNS A record → this project's static IP.
10. Cert poll + smoke.

Direct mode is a full duplicate of the reseller one-time setup, per customer. The trade is billing isolation in exchange for recurring ingress cost borne directly by the end-customer.

## Dependencies the script needs

| Tool/credential | Why |
|---|---|
| Vendor CLI (authenticated as org admin) | All provisioning calls. |
| DNS provider API token (narrow scope: DNS edit on the root domain only) | Per-customer DNS record creation. |
| Repo-host API token (create repo + initial commit from template) | Repo provisioning. |
| YAML parser | Customer spec parsing. |

Credentials should live in a protected off-repo location (e.g., `~/.makemyweb/credentials`, chmod 600) and be referenced by env var from the scripts.

## Edgar's battle-test findings (all covered by the pipeline)

1. ✅ Two non-obvious IAM bindings required on every new project.
2. ✅ Deploy-uploader and image-builder each have their own ignore file; author both.
3. ✅ Deploy-context should also exclude test-harness config (vitest setup, etc.) or the framework's build-time typecheck fails on imports that were correctly stripped from the image.
4. ✅ `.env.example` must not ship optional keys as blank strings.
5. ✅ DNS records for shared-ingress customers point to the LB's static IP, not to vendor-hosted gateway hostnames.
6. ✅ Vendor's preview-tier domain-mapping feature is NOT the current recommended path; production is LB-fronted.
7. ✅ Local `typecheck → lint → test → build` chain should be encoded in the CI pipeline so remote builds never fail on errors a local pre-check would catch.

## Script shape (when implemented)

```
makemyweb-infra/
├── customers/<slug>.yaml        # per-customer spec, committed
├── scripts/
│   ├── onboard.sh <slug>.yaml   # branches on billing mode; runs full pipeline
│   ├── teardown.sh <slug>.yaml  # reverses it
│   └── add-domain.sh            # cert update + URL-map entry + DNS only
└── credentials/                 # off-repo, chmod 600
```

## Out of scope for Step 52

- Dashboard UI wrapping the scripts (future Step 53).
- Payment-processor integration for reseller invoicing (business-layer, not infra).
- Multi-stack Docker templates beyond Next.js (future, when second template lands).
- Automated migration of existing customer projects between modes.

## Promotion plan once implemented

1. Build `onboard.sh` against Edgar's reseller-mode spec; confirm it re-runs cleanly after any step.
2. Provision a second customer end-to-end without human intervention beyond YAML editing.
3. Promote the scripts to framework: they live at the template level (stack-specific) or in a new `dist/deploy/` folder, depending on where they end up being stack-agnostic vs stack-specific.
4. Log promotion as a dated Step in `CHANGELOG.md`.
5. Re-sync downstream projects via `archetype/update.sh`.
