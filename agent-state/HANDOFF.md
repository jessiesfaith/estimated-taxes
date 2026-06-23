# HANDOFF

_Last updated: 2026-06-23_

## Active request status
**COMPLETE.** Cleaned, deployed, and handed off. Nothing mid-flight.

## What this tool is now
Single-file (`index.html`, ~195 KB, vanilla HTML/CSS/JS, no build) estimated-tax & safe-harbor calculator.
Covers, for **federal + all 50 states + DC**:
- **Income:** W-2/paystub sources (project mid-year / YTD / W-2), 1099 self-employment (SE tax, mileage),
  Social Security benefits (Pub-915 taxation; FICA + excess-SS credit), investment income (interest, ordinary &
  qualified dividends, LT & ST capital gains), passive rental (Schedule E, US **+ Mexico** ISR shown separately),
  Schedule K-1 (partnership/S-corp/trust), and an NOL carryforward.
- **Federal calc:** progressive brackets, **preferential 0/15/20% LTCG + qualified-dividend rates** (Qual Div & Cap
  Gain Worksheet), **§199A QBI deduction**, 3.8% NIIT, 0.9% additional Medicare, SS taxation, $25k passive-loss
  allowance, §1211 capital-loss cap, §172 NOL (80% limit; cascades into AGI → SS taxability + NIIT MAGI).
- **State calc:** generic state engine; each state has brackets/std/exemption/SS-treatment/cap-gains-exclusion/surtax.
- **Output:** 90/100/110% safe-harbor target, refund/shortfall, per-paycheck adjustment, quarterly schedule + due
  dates, full IRS + state progressive-rate breakdown ("show the math"), PDF + CSV download, self-test button.
- Public page is **self-contained & unbranded** (no Fast Insights links/branding; repo docs not served — see below).

## Architecture (where things live in index.html)
- **Tax constants:** `TAX_BY_YEAR[2026]` (federal + `ca` blocks); roll a new year by copying the block.
- **State engine:** `STATES` registry (CA via getters on `TAX.ca`; 9 no-tax states; `''` = Other/federal-only) +
  `stateTax()`. The other 49 states load from a `STATE_DATA` literal via `buildStates()` (top bracket upTo→Infinity,
  hoh/mfs default to single, generic 80% NOL, default safe harbor). `populateStates()` builds the dropdown.
- **Pure tax fns (audited by `selfTest()`):** bracketTax, federalTax, federalPreferentialTax, qbiDeduction,
  nolDeduction, netCapitalGain, taxableSocialSecurity, rentalAllowedNet, netInvestmentIncomeTax, selfEmploymentTax,
  additionalMedicareTax, excessSocialSecurityCredit, mexicoRentalTax, stateTax, safeHarbor, estSchedule.
- **Engine entry:** `calculate()` → `render()` + worksheets (`fedWorksheet`/`caWorksheet`) + `buildCsv`/`buildReportHtml`.

## How this was verified
- `selfTest()` (the "Run self-test" button): **49/49 pass**. Run it after any edit.
- Multiple adversarial multi-agent reviews this session (SS/rental/K-1; LTCG/QDI+QBI; NOL; NOL→AGI cascade;
  state-data accuracy across all 41 income-tax states). All confirmed findings were fixed.
- California is **byte-identical** to the original (MFJ $80k → fed $5,240 / CA $1,173) — the regression guard.

## Known limitations / approximations (also in the in-app disclaimer)
- **State estimates are approximate:** no local/city income taxes (NYC, OH/PA municipalities, MD counties, etc.);
  some state-specific retirement/cap-gains rules and exemptions/credits not modeled; SS-taxing states use the federal
  taxable-SS amount; figures are 2026 or latest-published (2025) proxies. A few states were medium/low-confidence in
  the data review — refine a specific state on request.
- **Federal simplifications:** QBI W-2/UBIA limit above the threshold not modeled (above-threshold non-SSTB = optimistic
  upper bound); §1250/collectibles/QSBS special gain rates not modeled; carryforwards (passive/capital/NOL) flagged but
  not tracked across years; Mexican ISR shown separately with **no** US/state foreign-tax credit (per Jessica's choice).
- CA exemption credit kept at the tool's existing 149/298 (research shows 2025 = 153/306) — left for consistency.
- Minor: `a_ca_exempt` (CA exemption) isn't re-seeded by `clearAll()`; changing filing via the dropdown reseeds it.

## To add or update a state (maintenance)
Edit the `STATE_DATA` object in index.html (search `const STATE_DATA`). Each entry:
`{code,name,taxesSS,capGainsExclusionPct,brackets:{single,mfj,hoh,mfs},std:{...},exemptionCredit?:{...},surtax?:{threshold,rate}}`.
Brackets: `{upTo,rate}` (rate decimal, top `upTo:999999999`). For states starting from **federal taxable income**
(CO, SC, ID, MT, ND, NM, MO, IA), set `std` to the federal standard deduction (16100/32200/24150/16100). Then run the
self-test and spot-check a $100k single filer. No-tax state → `{code,name,noTax:true}`.

## Deployment
- **Git-connected push-to-deploy.** `git push origin main` → Vercel auto-deploys (verified repeatedly this session).
  Remote `origin = git@github-jessica:jessiesfaith/estimated-taxes.git` (SSH alias `github-jessica`; this is Jessica's
  repo — the machine's HTTPS cred is Chris's and cannot push here).
- **Production:** https://estimated-taxes.vercel.app (200, public) + https://app.fastinsights.io/estimated-taxes
  (proxied by the AR-Tool-Beta / fast-insights-app project — already wired).
- **Privacy/leak control:** `.vercelignore` excludes the paystub PDF, `*.md`, `CLAUDE.md`, `README.md`, `agent-state`,
  dotfiles — so only `index.html` is publicly served (the shared link is a single self-contained page). Keep it that way.

## Local dev
`npx http-server "10 Code Repos/Estimated_taxes" -p 8124` (or use the `.claude/launch.json` "estimated-taxes" config /
preview tools). Open the page, click **Run self-test**, use **Demo** to load a full sample.

## Next recommended actions
- Optional polish: refresh CA exemption credit to 153/306; reseed `a_ca_exempt` in `clearAll()`; add Open Graph meta
  tags so the LinkedIn link unfurls with a title/description/image; tighten any medium-confidence state on request.
- Optional product: wire the (currently cosmetic) reminders/lead form to a real backend; stand up
  `estimatedtaxes.jessicadougherty.com` as an additional Vercel domain for a branded share link.

## Safe to start a new session?
**Yes.** Clean, all tests pass, deployed and live, nothing mid-flight.
