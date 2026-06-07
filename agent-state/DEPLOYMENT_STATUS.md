# DEPLOYMENT_STATUS

_Last updated: 2026-06-07 - verified via Vercel MCP + live HTTP_

## Provider
**Vercel** (CLI deploys; project linked locally via `.vercel/`). Deploys the static folder
(`index.html`). Framework = null (no build). **Not** connected to GitHub -> no git-push auto-deploy.
- Project: `estimated-taxes`
- Project ID: `prj_zhvIMpwg9wk2LEWpQFui4QvGxtCt`
- Team/org ID: `team_nctazuLdYORXTnrDm0PWCIJg`

## Live URL(s)
- **https://estimated-taxes.vercel.app** (canonical, **200 / public**).
- https://app.fastinsights.io/estimated-taxes (**200** - proxied by the fast-insights-app project).
- https://estimated-taxes-jessica-dougherty-s-projects.vercel.app (**401** - Deployment Protection on
  the scoped alias; the canonical domain above is public).

## Last deploy
- Latest production deployment `dpl_9nZbHRDW3Ro8coCMdZQDVtoPckTA` = **READY** (created 2026-06-07).
  All 10 listed deployments are READY. Deployed by the Vercel CLI (actor = claude-code agent); no
  GitHub commit metadata (CLI deploys, not git-push).
- Local HEAD = single commit `642f95e`; working tree clean, so the deployed folder matches HEAD.

## Environment variables
- **None** - fully client-side static tool (no API keys, no backend).

## Deploy command
- **Primary:** `npx vercel --prod` from the repo root (global `vercel` CLI not installed - use `npx`).
- There is **no** git-push auto-deploy. To add it: connect a GitHub repo + the Vercel project (a
  GitHub operation - confirm Chris vs Jessica first).

## Deploy verification command
- Vercel MCP: `list_deployments` / `get_project` (check `readyState` == `READY`).
- URL: `curl -I https://estimated-taxes.vercel.app` (expect 200).

## Production verification checklist
- [x] https://estimated-taxes.vercel.app returns 200 (public).
- [x] app.fastinsights.io/estimated-taxes returns 200 (proxy).
- [ ] Tool loads; inputs compute estimated tax + safe-harbor correctly.
- [ ] Confirm no personal PDF/image was deployed (vercelignore covers `*.pdf` + the paystub).

## Current status
- **VERIFIED - READY + public.** Production live at https://estimated-taxes.vercel.app (200). No
  pending deploy. Risk: low (static). Deploys are **manual via CLI** - run `npx vercel --prod` after edits.
- Note: Vercel project flag `live:false` = traffic-observability off, not an outage.

## Status legend
verified [OK] | failed [FAIL] | pending [pending] | not-configured | not-requested | requires-manual-verification
-> **Current: verified [OK] (prod READY + 200; CLI/manual deploy)**
