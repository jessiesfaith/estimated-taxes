# CURRENT_STATE — Estimated Taxes (Estimated Tax & Safe-Harbor Tool)

_Last updated: 2026-06-24_

## Purpose
Single-page static tool that estimates **federal + all 50 states (+ DC)** income tax and the **safe-harbor**
quarterly target (to avoid the IRS underpayment penalty), across many income types (W-2/paystub, 1099/SE, Social
Security, investment income, rental incl. Mexico, K-1, with QBI/NIIT/NOL). Calculator is fully client-side — tax
figures never leave the browser. Adds an in-app **Field Guide** (right pane, per-field help) and a **live reminders
signup** (captures opt-in email/phone via Formspree → info@fastinsights.io). **Full map: `agent-state/HANDOFF.md`.**

## Stack
- **Framework / package manager:** none (vanilla HTML/CSS/JS, no build).
- **Key file:** `index.html` — the entire tool (inlined CSS + JS + 50-state data + field-guide JSON). Verify with the in-app self-test.

## Commands
| Purpose | Command |
|---|---|
| Local dev | `npx http-server . -p 8124` → http://localhost:8124 (python not installed) |
| Build / lint | none |
| Test | in-app **Run self-test** = **45 cases**, all must pass |
| Deploy (prod) | `git push origin main` → Vercel auto-deploys (git-connected) |

## Key behavior (current)
- Federal + chosen-state estimate; live recompute on input/state/filing change; the state selector retitles every label.
- Income: W-2/paystub (project / YTD / W-2), 1099/SE (+mileage, SE tax), Social Security (Pub-915), investment
  (interest / dividends / LTCG-STCG at preferential rates), rental (Schedule E + Mexico ISR), K-1, QBI, NOL.
- Output: 90/100/110% safe-harbor target, refund/shortfall, per-paycheck adjustment, quarterly schedule + due dates,
  full progressive-rate breakdown, PDF/CSV.
- **Field Guide:** right-pane panel (📘 header button + floating button) explaining all 75 fields (broad / example /
  granular), with search + focus-sync; content lives in a `<script type="application/json" id="guideData">` block.
- **Privacy:** calculator 100% client-side (tax figures never sent); only persistence is the optional employer-layout
  localStorage (`fi_estimated_tax_templates_v1`, never amounts). Banner + footer disclaimer + analytics note on the page.
- **Reminders signup:** `submitLead()` POSTs to Formspree `https://formspree.io/f/mnjrbgzp` → info@fastinsights.io,
  capturing opt-in email/phone + consent flags (due-date / SMS / marketing / CPA). **Capture only — automated SENDING
  of reminders is NOT built** (future: cron + email/SMS, e.g. Supabase Edge Function + Resend for email, Twilio for SMS).

## Git
- Branch `main`; remote `origin = git@github-jessica:jessiesfaith/estimated-taxes.git` (SSH alias `github-jessica`,
  Jessica's key — the machine's HTTPS cred is Chris's and can't push here). **Push-to-deploy connected.** Latest commit: `git log -1`.

## Deploy
- **Git-connected Vercel** (`git push origin main` → auto). Prod: https://estimated-taxes.vercel.app (200) +
  https://app.fastinsights.io/estimated-taxes (proxied by the fast-insights-app project).

## Known notes / risks
- Privacy-critical: personal docs (paystubs/PDFs/images) are git+vercel-ignored — never commit/deploy them
  (`.vercelignore` ships only `index.html`).
- No automated tests beyond the in-app 45-case self-test — run it after edits.
- Reminders are capture-only; do a live test signup; if a cross-domain POST bounces, allow `estimated-taxes.vercel.app`
  in the Formspree form settings.
- OneDrive Files-On-Demand: `.git`/files may be cloud placeholders — retry reads once.

## Repo
- Path: `01 Projects/Fast Insights/10 Code Repos/Estimated_taxes` (inside the Builder_OS vault)
- Remote: `github.com/jessiesfaith/estimated-taxes` · Branch: `main`
