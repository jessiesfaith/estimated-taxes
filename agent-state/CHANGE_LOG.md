# CHANGE_LOG

Newest first. Keep entries to a few bullets - no long logs.

## 2026-06-22 - Add preferential LTCG/QDI rates + §199A QBI deduction (federal); CA keeps ordinary
- **Files:** `index.html` only.
- **What:** Two federal-only additions feeding the same engine, with CA divergence handled by construction:
  - **Preferential rates on long-term capital gains + qualified dividends** via the IRS Qualified Dividends &
    Capital Gain Tax Worksheet (0/15/20% stacking on top of ordinary income). New **Investment income** section
    (interest, ordinary + qualified dividends, net LT + ST gain) and a K-1 **qualified-dividends split** (box 6b);
    K-1 box 9a now treated as long-term. Schedule-D netting (`netCapitalGain`) with the $3k/$1.5k-MFS loss cap.
  - **§199A QBI deduction** — 20% of QBI (1099 net **less ½ SE tax** + K-1 box-1 ordinary), capped at 20% of
    (taxable income − cap gains − qual. dividends), $400 OBBBA minimum, SSTB phase-out; below the std deduction,
    reduces federal taxable income only. New QBI section (enable / override / SSTB toggle).
  - **California:** unchanged by construction — taxes all gains/dividends as ordinary, allows no QBI, no NIIT;
    `caAgi` never sees the pref/QBI variables. NIIT base uses gross investment income; AGI is unaffected by QBI.
- **Engine:** constants `federal.ltcg` + `federal.qbi` (2026 from Rev. Proc. 2025-32); pure fns `netCapitalGain`,
  `federalPreferentialTax`, `qbiDeduction`. `federalPreferentialTax(pref=0)==bracketTax` so pure-W-2 users are byte-identical.
- **Process:** 4-agent research/design pass (verified 2026 figures + algorithms + CA conformity) → build → 4-dimension
  adversarial review (pref-stacking, QBI, CA/NIIT/AGI, double-counting). Review caught + FIXED one bug: QBI base wasn't
  reduced by the §164(f) deductible half of SE tax (overstated the deduction) — now `(seNetProfit − halfSE) + k1Ord`.
- **Verification:** 45/45 self-tests pass. Integration ties out: single $50k+$40k LTCG → pref $40k split $15,550@0% +
  $24,450@15%, fed tax $7,488; QBI $50k-SE base correctly net of half-SE; pure-W-2 MFJ $80k unchanged ($5,240). No console errors.
- **Known simplifications (in disclaimer):** QBI W-2/UBIA limit above threshold not modeled (above-threshold non-SSTB =
  optimistic upper bound); §1250/collectibles/QSBS special rates not covered; carryforwards not tracked across years.

## 2026-06-22 - Add Social Security, passive rental (incl. Mexico), and Schedule K-1 income
- **Files:** `index.html` only (single-file tool).
- **What:** Four new collapsible input sections feeding the same federal+CA engine:
  - **Social Security** - benefits received taxed up to 85% federally via the Pub-915 provisional-income
    worksheet; **excluded from CA income** (caAgi = fed AGI - taxable SS). Plus FICA-withheld tracking that
    flags **excess Social Security tax** (2+ employers) as a refundable federal credit.
  - **Passive rental (Schedule E)** - per-property cards (rents - expenses - depreciation); not SE-taxed;
    net loss limited by the **$25k active-participation allowance** with $100k-$150k MAGI phaseout
    (suspended remainder flagged as carryforward); positive net + portfolio income drives the new **3.8% NIIT**.
  - **Mexico property** - per-property toggle; estimates **Mexican ISR = 25% of gross rent** (editable),
    shown in its own results card **separately, no FTC** (rent still in US+CA worldwide income; CA gives no
    foreign credit). Chosen per Jessica: 25%-of-gross method, show-separately.
  - **Schedule K-1** - per-K-1 cards (ordinary box 1, rental box 2, guaranteed pmts, interest, dividends,
    cap gain); per-K-1 "subject to SE tax" toggle (general partner) adds to the SE base; portfolio items feed NIIT.
- **Engine:** new pure fns `taxableSocialSecurity`, `rentalAllowedNet`, `netInvestmentIncomeTax`,
  `mexicoRentalTax`, `excessSocialSecurityCredit`; constants `federal.ssTaxation/niit/fica`, top-level `rental`/`mexico`.
- **Verification:** 33/33 self-tests pass (16 new). Demo end-to-end ties out: fed AGI $211,858, CA AGI $191,458
  (= fed - $20,400 taxable SS), NIIT $451, Mexico ISR $4,500. Edge cases (empty state, $30k loss fully suspended
  at $300k income) clean; no console errors. Verified via local http-server preview on :8124.
- **Pre-deploy adversarial review (6 dimensions, 9 agents)** caught + FIXED 3 medium bugs before push:
  (1) K-1 box-2 rental loss was borrowing Schedule E's active-participation flag -> now K-1 rental runs as
  passive (limited-partner default), losses suspended (§469(i)(6)(C)); (2) a negative K-1 capital gain
  deducted unlimited against ordinary income -> now floored to -$3,000 / -$1,500 MFS with carryforward shown
  (§1211(b)); (3) 0.9% Additional Medicare liability had no matching employer-withholding credit -> now credits
  0.9% per-job wages over $200k (Form 8959 Part IV), so high-wage single-employer W-2 users no longer show a
  phantom shortfall. SS/CA-exclusion/NIIT dimensions reviewed clean. Re-verified: all fixes assert correctly,
  pure-W-2 + demo unchanged.
- **Known simplifications (told to user):** cap gains taxed as ordinary; no QBI/AMT/most credits; passive-loss
  carryforward not tracked across years; Mexico IVA not modeled; no US foreign tax credit for Mexican ISR.
- **Deploy:** NOT pushed/deployed (no user request). `git push` would auto-deploy via the git-connected Vercel project.

## 2026-06-07 - Initialize agent-state workflow for Estimated_taxes
- **Files:** `CLAUDE.md` + `agent-state/*` (8 files). All ASCII.
- **Reason:** Bring the W-2 Estimated Tax tool into the durable-state workflow (skipped in the bulk
  run because its `.git` was a OneDrive-dehydrated placeholder, mis-read as "not a git repo").
- **Findings:** IS a git repo (branch `main`, 1 commit `642f95e`, clean, no remote). Vercel project
  `estimated-taxes`, CLI-deployed, prod READY + public (estimated-taxes.vercel.app = 200).
- **Verification:** deploy verified via Vercel MCP + live 200; no app code changed.
- **Git:** ran `git init` = harmless no-op reinit on the existing repo. **Deployment:** unchanged.
- **Obsidian:** target recorded (lazy create on first /cleanup-handoff).
