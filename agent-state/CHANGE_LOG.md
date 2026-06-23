# CHANGE_LOG

Newest first. Keep entries to a few bullets - no long logs.

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
