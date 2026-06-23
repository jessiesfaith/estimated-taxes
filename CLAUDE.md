# Estimated Taxes (W-2 Estimated Tax & Safe-Harbor Tool) -- repo notes

Generic workflow rules live in `~/.claude/CLAUDE.md` (they apply to every repo). This file holds
**repo-specific facts only**. Source of truth = this repo + `agent-state/*.md`, not the chat transcript.

## Repo facts (verified 2026-06-23)
- **What:** Single-page static estimated-tax & safe-harbor tool. Now covers **federal + all 50 states + DC** and
  many income types: W-2/paystub, 1099 self-employment, Social Security, investment income (interest/dividends/cap
  gains with preferential LTCG/QDI rates), passive rental (US + Mexico ISR), Schedule K-1, plus QBI, NIIT, addl
  Medicare, and NOL carryforward. Generic state engine (`STATES` + `STATE_DATA`/`buildStates`). All client-side; no
  accounts; nothing uploaded. Public page is self-contained/unbranded. **See `agent-state/HANDOFF.md` for the full map.**
- **Package manager / framework:** none. Vanilla HTML/CSS/JS, no `package.json`, no build step.
- **Key file:** `index.html` (~195 KB -- the entire tool: inlined CSS + JS + state data). Verify edits with the
  in-app **Run self-test** button (49 cases).
- **Dev (localhost):** `npx http-server . -p 8124` (python is not installed on this machine) -> http://localhost:8124.
- **Build / test / lint / typecheck:** none configured. Verification = manual + URL check.
- **Git:** branch `main`, remote `origin` = **github.com/jessiesfaith/estimated-taxes** (public).
  Pushed via SSH host alias `github-jessica` (key `~/.ssh/jessiesfaith_ed25519`); the repo's
  `core.sshCommand` is pinned to Windows OpenSSH. The machine's HTTPS credential is Chris's
  (`dogsleddev`) and CANNOT push here -- this is Jessica's repo.
- **Deploy:** **`git push` -> auto-deploy.** The `estimated-taxes` Vercel project is git-connected to
  the repo above, so pushing to `main` auto-deploys to production (verified). Manual fallback:
  `npx vercel deploy --prod --yes` from the repo.
- **Production:** https://estimated-taxes.vercel.app (200, public) -- also reachable via
  https://app.fastinsights.io/estimated-taxes (proxied by the fast-insights-app project). Vercel
  project `estimated-taxes`. (The `-jessica-dougherty-s-projects` scoped alias returns 401 /
  Deployment Protection; the canonical `.vercel.app` domain is public.)
- **PRIVACY:** personal documents (`Paycheck_2026-05-31.pdf`, any `*.pdf/*.png/*.jpg`) are BOTH
  gitignored and vercelignored -- never commit or deploy them. Keep it that way.

## agent-state/ files (durable memory)
`CURRENT_STATE.md` | `ACTIVE_REQUEST.md` | `VERIFICATION_STATUS.md` | `DEPLOYMENT_STATUS.md` |
`CHANGE_LOG.md` | `HANDOFF.md` | `NEXT_SESSION_PROMPT.md` | `OBSIDIAN_SYNC.md`.
If any is missing, recreate it with the same headings the others use.

## Environment notes (this machine)
- **OneDrive Files-On-Demand:** this repo is inside a OneDrive-synced Obsidian vault. Files (incl. the
  `.git/` folder) may be cloud placeholders that tools intermittently fail to see -- that is why an
  earlier scan wrongly reported "not a git repo". Retry once; `git` itself reads it fine.
- **GitHub = Jessica (`jessiesfaith`).** This repo + its Vercel project are Jessica's. The machine's
  stored HTTPS credential is Chris's (`dogsleddev`) and can't push here, so pushes use a dedicated SSH
  key for jessiesfaith (host alias `github-jessica`). Before any GitHub work here, see the
  `github-identity` skill (or just confirm: Chris or Jessica?).
- Obsidian notes live in the vault, NOT in this repo -- see `agent-state/OBSIDIAN_SYNC.md`
  (target: `01 Projects/Fast Insights/00 Command Center/Agent State/Estimated_taxes/`).
