# Step 52 (pending) — Customer Onboarding & Deployment Automation

Trigger: Edgar's first manual Cloud Run deploy (2026-04-22) surfaced the full list of steps, IAM bindings, config files, and DNS work required per customer site. Several of those steps are ergonomic footguns that must be encoded in automation so subsequent customers don't re-discover them.

## Scope

One declarative spec per customer → one command → fully provisioned customer environment, code repo, CI/CD, custom domain, SSL.

## Input: `customers/<name>.yaml`

```yaml
name: edgar
subdomain: edgar                  # becomes edgar.makemyweb.site
stack: nextjs                     # reserved — future multi-stack support

gcp:
  folder_parent: customers        # folder under org
  project: edgar-prod
  region: us-central1

billing:
  mode: reseller                  # or "direct"
  master_billing_account: 01XXXX-YYYYYY-ZZZZZZ  # used if mode=reseller
  customer_billing_account: null  # populated by customer form if mode=direct

repo:
  github_org: d3r3nic
  name: Edgar-website
  branches:
    main: dev
    staging: staging
    production: prod              # maps to unsuffixed Cloud Run service

env:
  NEXT_PUBLIC_WP_GRAPHQL_URL: https://cms.edgar.com/graphql
  NEXT_PUBLIC_CONTACT_EMAIL: hello@edgar.com
  NEXT_PUBLIC_WP_REST_URL: null   # optional — omit rather than empty-string
  WP_ADMIN_TOKEN: null            # optional, goes into Secret Manager

features:                         # dashboard-toggleable flags
  blog: true
  products: false
  auth: false
  monitoring: true
  min_instances: 0
```

## What `onboard.sh <customer.yaml>` must do

Each step is idempotent — re-running after partial failure resumes correctly.

### 1. GCP provisioning
- Create folder `customers/<Name>/` under org `makemyweb.site` (skip if exists)
- Create projects: `<name>-prod`, `<name>-staging`, `<name>-dev` under folder
- Link billing account (reseller master OR customer-supplied)
- Enable APIs: `run`, `cloudbuild`, `artifactregistry`, `iam`, `secretmanager`
- Grant IAM bindings (both are NOT auto-granted on post-2024 projects):
  - Org admin → `roles/owner` on each project
  - Compute default SA → `roles/cloudbuild.builds.builder`

### 2. GitHub repo
- Create from `headless-wp-next` template (Octokit)
- Clone template's `apps/reference-site/` as initial content
- Vendor latest `@template/*` tarballs into `vendor/template-packages/`
- Rewrite `package.json` deps + `pnpm.overrides` to point at `file:./vendor/...`
- Drop in `Dockerfile`, `.dockerignore`, `.gcloudignore` (already template-resident as of this Step)
- Generate `.env.example` with customer's required keys filled, optional keys COMMENTED (not empty)
- Create three branches: `main`, `staging`, `production`

### 3. Cloudflare DNS
- Read zone ID for `makemyweb.site` from config
- For each environment: create CNAME `<env>.<subdomain>.makemyweb.site → ghs.googlehosted.com`
  - Prod is bare `<subdomain>.makemyweb.site`
  - Staging is `staging.<subdomain>.makemyweb.site`
  - Dev is `dev.<subdomain>.makemyweb.site`
- **Critical**: `proxied: false` on every record. Cloudflare proxy breaks Cloud Run's ACME cert issuance.
- One-time per root domain: ensure Search Console TXT verification exists. If not, create + verify.

### 4. Cloud Run + domain mapping
- Initial deploy of each service (from the newly-seeded repo)
- Create domain mappings (`gcloud beta run domain-mappings create`) for each env
- Poll cert status until `CertificateProvisioned: True` (5–60 min typical)

### 5. CI/CD triggers
- Cloud Build trigger per branch:
  - `main` → `<name>-dev` service
  - `staging` → `<name>-staging` service
  - `production` → `<name>` service (prod)
- `cloudbuild.yaml` in repo defines: typecheck → lint → test → build → deploy
- Substitutions: `$_SERVICE`, `$_ENV`, set per trigger

### 6. Secrets
- Any `WP_ADMIN_TOKEN` or similar → Cloud Secret Manager
- Cloud Run service references secret, not env var value

### 7. Output
- Prints:
  - Cloud Run URLs (`.run.app`)
  - Custom domain URLs (may say "pending cert")
  - GitHub repo URL
  - Cloud Build console links per trigger

## Dependencies the script needs

| Tool | Why |
|---|---|
| `gcloud` CLI (authed as org admin) | All GCP operations |
| Cloudflare API token (Zone:DNS:Edit scoped to `makemyweb.site`) | DNS records |
| GitHub personal access token (repo create + branch protect) | Repo provisioning |
| `yq` or equivalent | Parse customer YAML |

Store tokens in a `.env` next to the script (gitignored) or in Cloud Secret Manager.

## Edgar's post-deploy gotchas (all must be covered by the script)

1. ✅ Two IAM bindings that aren't auto-granted — captured in step 1
2. ✅ `.gcloudignore` required (distinct from `.dockerignore`) — template-resident
3. ✅ Vitest setup files must be excluded from build context — template-resident
4. ✅ `.env.example` must comment-out optional keys, not leave blank — template-resident
5. ✅ Cloudflare CNAME must be `proxied: false` — captured in step 3
6. ✅ Search Console domain verification distinct from Cloud Identity TXT — captured in step 3
7. Local pre-deploy verification (`pnpm typecheck && build`) — encoded in `cloudbuild.yaml` so Cloud Build also catches it

## Promotion plan

1. Build `onboard.sh` against Edgar's manual run (parameterize from `edgar.yaml`).
2. Add a second customer via `./onboard.sh customers/<new>.yaml` — confirm it runs unattended.
3. Promote to framework:
   - `dist/templates/deploy/nextjs/cloudbuild.yaml` (build pipeline)
   - `dist/scaffolding/SCAFFOLD-DEPLOY.md` (phase doc if new)
   - OR extend `dist/scaffolding/SCAFFOLD-FRONTEND.md` with a Deployment section
4. Log Step 52 in CHANGELOG.md.
5. Deploy update to existing projects via `update.sh`.

## Out of scope for Step 52

- Stripe-based reseller billing (future Step 53+): current reseller mode just uses master GCP billing account; customer invoicing happens out of band.
- Multi-stack Docker templates (Django, .NET): Nextjs only for this Step. Stack field in YAML is reserved.
- Dashboard UI: this Step ships the bash/script layer only. Dashboard sits on top (separate Step).
