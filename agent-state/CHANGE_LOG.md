# CHANGE_LOG

Newest first. Keep entries to a few bullets - no long logs.

## 2026-06-23 - State change recomputes the form + state-aware Assumptions panel
- Selecting a state in the Step-2 dropdown now recomputes the results immediately (was: required a Calculate click;
  only the results-banner selector auto-recalced).
- Assumptions panel is now state-aware: title, the "California" column header, and the source note follow the selected
  state; the state std-deduction column shows that state's built-in figures (read-only) for non-CA and the FTB/CA-only
  rows (exemption credit, CA 110% threshold) are hidden; California stays fully editable. `readAssumptions()` is guarded
  to only write `TAX.ca` when California is selected (so a non-CA state's read-only figures can't corrupt CA's config).
- Verified: CA→NY→TX→CA switching updates result card + tax + panel correctly; CA byte-identical (MFJ $80k → $1,173);
  49/49 self-tests; no console errors.

## 2026-06-23 - Clean + handoff
- Removed dead constant (`mexico.mxnPerUsd`, unused). Updated copy: header "Federal + all 50 states", Step-2 state
  note, and the disclaimer (state estimates approximate; no local/city taxes / some state items not modeled).
- Refreshed `HANDOFF.md` (full architecture + maintenance map), `CURRENT_STATE.md`, `CLAUDE.md` (was stale at "W-2 only").
- Removed OS-temp scratch scripts used to generate/inject/fix the state data (outside the repo).
- Verified: 49/49 self-tests pass, demo + Mexico ISR intact, no console errors. Shipped via git push (auto-deploy).

## 2026-06-23 - Populate all 50 states + DC (multi-state step 3)
- **Files:** `index.html` (added a `STATE_DATA` block + `buildStates()` loader after the STATES registry).
- **What:** All 50 states + DC now have a state estimate. Figures (brackets, std deduction, exemptions, SS taxation,
  cap-gains exclusions, surtaxes) were researched (5-agent web-search workflow, 2026 or latest-published), transformed
  to the engine schema programmatically (Node — no hand-transcription), and loaded via `buildStates()` (top bracket
  upTo→Infinity, hoh/mfs default to single where a state has no separate schedule, generic 80% NOL, default safe harbor).
- **Accuracy review:** a 5-agent adversarial review checked all 41 income-tax states against authoritative sources +
  the engine's federal-AGI base model; it flagged 13 with material errors, all FIXED (18 corrections):
  CO & SC (start from federal taxable income → std set to the federal standard deduction), HI (2026 std doubling under
  Act 46), KS/LA/MD/ME/MS/OK/VT/DC (HOH/MFS std and OK brackets), IA (HOH exemption credit), MT (HOH brackets).
- **Verification:** 49/49 self-tests pass; spot-checks recompute correctly (CO $100k single → $3,692 not $4,400;
  HI → $5,883; NY → $4,860); **California byte-identical** (MFJ $80k → fed $5,240 / CA $1,173); no console errors.
- **Disclaimer updated:** state estimates are approximate — local/city taxes and some state-specific
  deductions/credits/retirement rules aren't modeled. (CA exemption credit left at the tool's existing 149/298.)

## 2026-06-23 - Generalize to a state-driven engine + state selector (step 1 of multi-state)
- **Files:** `index.html`.
- **Why:** the tool is being marketed nationwide; it was California-only and showed CA numbers to everyone.
- **What (step 1):** Replaced `californiaTax()` with a generic `STATES` registry + `stateTax()` engine.
  California reuses its existing editable, year-keyed `TAX.ca` block via getters (so CA is byte-identical).
  Added a **state selector** (Step 2 + a quick-select on the results banner), seeded with California, the
  9 no-income-tax states, and an "Other / not added yet (federal only)" option. Non-CA/no-tax selections show
  a clean "no state income tax" result and federal-only — fixing the bug where everyone saw California tax.
  All state-facing render/worksheet/CSV/PDF labels are now dynamic (state name, no-tax branch). The state base
  is the PRE-federal-NOL non-SS income (states figure their own NOL) — caught/fixed during the refactor.
- **Verification:** 49/49 self-tests pass. CA regression byte-identical (MFJ $80k → AGI 80000 / fed 5240 / CA 1173 /
  CA taxable 68588; demo CA taxable 205352). Texas/Other → $0 state, federal unchanged ($231,458/$43,635 across switch).
  No console errors. (A JS parse error from an over-escaped apostrophe was caught in verification and fixed.)
- **Next (step 3):** populate the ~41 income-tax states from the gathered 2026/latest figures (research done),
  verify per state, adversarial review, ship. Engine already supports flat/progressive brackets, SS taxation,
  cap-gains exclusions, surtaxes, and state NOL via the STATES config.

## 2026-06-23 - Make the public page self-contained / unbranded for direct sharing
- **Files:** `.vercelignore`, `index.html`.
- **Why:** Jessica wants to share the bare `estimated-taxes.vercel.app` link (e.g. on LinkedIn) with NO route to the
  Fast Insights hub or other tools, and "the only page people get" should be the tool itself.
- **What:**
  - **Leak closed:** `CLAUDE.md` and `agent-state/CURRENT_STATE.md` were being served publicly (HTTP 200) on the
    `.vercel.app` domain — they reveal the other tools + deploy internals + heavy "Fast Insights" branding. Added
    `*.md`, `CLAUDE.md`, `README.md`, `agent-state` to `.vercelignore` so only `index.html` is served. (Personal PDF
    already excluded.) **Verify after deploy that .vercelignore is honored on the git-connected build.**
  - **Unbranded the page:** removed the 3 "Fast Insights" mentions in index.html — the top source comment, the PDF
    report footer ("Generated by the … Estimated Tax tool"), and the cosmetic `BUSINESS_EMAIL` constant (now a
    jessicadougherty.com placeholder; set the real lead inbox when wiring up capture). No `<a>` links exist in the
    tool, so there's no outbound navigation regardless.
- **Note:** the GitHub repo is public, so CLAUDE.md/agent-state are still readable there — this only stops them being
  served on the shared `.vercel.app` domain.

## 2026-06-22 - NOL cascades into AGI-based items (Social Security taxability + NIIT)
- **Files:** `index.html` only.
- **What:** The FEDERAL NOL carryforward now reduces AGI (a Schedule-1 adjustment), so it flows into Social
  Security taxability (lower provisional income) and the 3.8% NIIT MAGI — previously it only reduced taxable
  income at the bracket step. Implementation in calculate(): compute SS at the PRE-NOL provisional only to set
  the §172 80%-limit base, cap the NOL, then subtract it from non-SS income, RE-derive SS at the lower provisional,
  and set agi = (income − NOL) + taxable SS. NII stays gross (NOL reduces only the MAGI side). California unchanged
  (CA NOL still separate; CA excludes SS; no NIIT).
- **Verification:** 49/49 self-tests pass. Cascade confirmed: $250k rental + $30k SS + $100k NOL → NIIT $2,869→$0
  (MAGI drops below $200k); $50k rental + $30k SS + $40k NOL → taxable SS $25,500→$0 (provisional $65k→$25k).
  Pure-W-2 / no-NOL byte-identical. 4-dimension adversarial review: math/limit-base/cascade clean; caught + FIXED one
  DISPLAY reconciliation bug — the CA build-up ("Federal AGI − SS = California AGI") didn't tie once fed AGI went
  post-NOL, so added a "+ Federal NOL added back (CA figures its own NOL)" bridge line; CA column now reconciles.
- **Simplification (disclaimer):** full NOL reduces MAGI (the §1411 "applicable portion" nuance not modeled).

## 2026-06-22 - Full IRS+FTB progressive-rate schedule display + NOL carryforward
- **Files:** `index.html` only.
- **What:**
  - **Full progressive-rate schedule:** `bracketTableHTML` rewritten to list EVERY 2026 bracket for the filing status
    (income-not-reached brackets greyed out) for both Federal (IRS) and California (FTB), so users see the whole
    progressive structure; long-term gains + qualified dividends show as 0/15/20% rows layered on top (federal).
  - **NOL carryforward:** new `s_nol` input (Step 2); pure fn `nolDeduction` (80% §172 limit). Federal: reduces
    taxable income after the std deduction, before QBI (QBI income limit then uses the post-NOL figure). California:
    80% limit but **suspended for 2024–26 when income ≥ $1M** (SB 167/175); applied via a new optional `nol` param on
    `californiaTax` (defaults 0 → existing results unchanged). Remaining NOL shown as carryforward. NOL is applied at the
    taxable-income level only (does not recompute AGI-based NIIT/SS — documented simplification).
- **Engine:** constants `federal.nol` + `ca.nol`; `nolDeduction`; `californiaTax` gained trailing `nol` arg.
- **Verification:** 49/49 self-tests pass. Integration ties out: single $80k + $100k NOL → fed NOL $51,120 (80% of
  $63,900), CA NOL $59,435; $1.2M income → CA NOL suspended ($0), federal NOL still allowed; full federal schedule renders
  all 7 brackets; pure-W-2 MFJ $80k unchanged ($5,240 fed / $1,173 CA). 4-dimension adversarial review returned **0 findings**.

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
