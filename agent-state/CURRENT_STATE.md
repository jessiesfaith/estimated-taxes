# CURRENT_STATE - Estimated Taxes (W-2 Estimated Tax & Safe-Harbor Tool)

_Last updated: 2026-06-22_

## Purpose
Single-page static tool that estimates a W-2 earner's federal income tax and **safe-harbor** payment
thresholds (to avoid underpayment penalties). Fully client-side; no backend, no accounts, no data
leaves the browser.

## Stack
- **Framework:** none (vanilla HTML/CSS/JS).
- **Package manager:** none (no `package.json`).
- **Key file:** `index.html` (~101 KB - the entire tool).

## Commands
| Purpose | Command |
|---|---|
| Local dev | `python -m http.server 8000` -> http://localhost:8000 |
| Build / test / lint / typecheck | none configured |
| Deploy (prod) | `npx vercel --prod` (Vercel CLI - deploys the working folder) |

## Key behavior
- One page; income inputs -> estimated annual/quarterly federal + CA tax + safe-harbor target. All computation in JS.
- Income sections (each a collapsible card group): W-2/paystub sources, 1099 self-employment, **Social Security**
  (taxed up to 85% fed / 0% CA + FICA & excess-SS-credit tracking), **passive rental (Schedule E, US + Mexico)**
  with the $25k loss allowance + 3.8% NIIT, and **Schedule K-1** (partnership/S-corp/trust). Mexico rental also
  estimates Mexican ISR (25% of gross) shown separately. (Added 2026-06-22 - see CHANGE_LOG.)
- No persistence beyond the browser session.

## Git
- Branch `main`, **1 commit** (`642f95e`, "Initialize repo: W-2 estimated-tax & safe-harbor tool",
  2026-06-06). Clean working tree. **No remote** (local-only repo).

## Deploy
- **Vercel CLI** (`npx vercel --prod`), project `estimated-taxes`. NOT git auto-deploy (no GitHub
  connection). Production: https://estimated-taxes.vercel.app (200) + app.fastinsights.io/estimated-taxes.

## Known notes / risks
- **Privacy-critical:** personal docs (paystubs, PDFs, images) are git+vercel-ignored. Do not commit
  or deploy them. A real `Paycheck_2026-05-31.pdf` sits in the folder (ignored).
- No automated tests - manual verification only.
- Deploys are CLI/manual (no auto-deploy) - remember to `npx vercel --prod` after changes.
- OneDrive Files-On-Demand: `.git` and other files may be dehydrated placeholders; retry reads once.

## Repo
- Path: `01 Projects/Fast Insights/10 Code Repos/Estimated_taxes` (inside the Builder_OS vault)
- Remote: none yet
- Branch: `main`
