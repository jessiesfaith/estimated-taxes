# DEPLOYMENT_STATUS

_Last updated: 2026-06-24_

## Provider
**Vercel**, project `estimated-taxes`, **git-connected → push-to-deploy**. Static (`index.html`; framework null, no build).
- Remote: `git@github-jessica:jessiesfaith/estimated-taxes.git` (SSH alias `github-jessica`).
- `git push origin main` → Vercel builds a new Production deployment automatically.

## Live URL(s)
- **https://estimated-taxes.vercel.app** — canonical, **200 / public**.
- **https://app.fastinsights.io/estimated-taxes** — **200**, proxied by the fast-insights-app project.
- (`-jessica-dougherty-s-projects` scoped alias = 401 / Deployment Protection; the canonical domain is public.)

## Last deploy
- Latest = commit `7a42acd` (reminders live + privacy/analytics update), pushed 2026-06-23, auto-deployed. Verified live
  200 with the new markers served. Working tree clean & in sync with origin.

## Environment variables
- **None** in the page (client-side). The reminders form POSTs to Formspree (`/f/mnjrbgzp` → info@fastinsights.io); no secrets.

## Deploy command
- **Primary:** `git push origin main` (auto-deploy). Fallback: `npx vercel deploy --prod --yes` from the repo.

## Privacy / leak control
- `.vercelignore` ships only `index.html` (excludes the paystub PDF, `*.md`, `agent-state`, dotfiles). Keep it that way.

## Current status
- **VERIFIED — READY + public.** Push-to-deploy working. Risk: low (static).
