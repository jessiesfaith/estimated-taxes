# ACTIVE_REQUEST

_Last updated: 2026-06-07_

## Current active request
Initialize the agent-state workflow for Estimated_taxes (seed agent-state/ + CLAUDE.md; verify deploy).

## Acceptance criteria
- [x] agent-state/ seeded (8 files) from verified repo + Vercel state.
- [x] CLAUDE.md created (repo facts + pointer to ~/.claude/CLAUDE.md).
- [x] Deployment checked via Vercel MCP + live URL (READY + 200).
- [x] Confirmed it IS a git repo (earlier "not git" was a OneDrive false-negative).

## Status
- Implementation: complete
- Verification: complete (prod READY + 200; no app code changed)
- Deployment: unchanged (CLI-deployed; prod READY)
- Obsidian: target recorded (notes lazily created on first /cleanup-handoff)
- **Overall: COMPLETE**

---

## Most recent request (2026-06-23)
**Request:** Make the state NAME follow the selected state in every UI label/note (resolve the deferred cosmetic issue);
use the generic word "state" where it's easier/more correct, but prefer the state name.
**Acceptance criteria:**
- [x] All previously-hardcoded "California"/"CA" visible text retitles to the selected state (input notes, labels,
      table headers, worksheet label, live result summaries, Mexico note, PDF footer, bottom-line).
- [x] Behavior-dependent claims stay correct (SS note branches on `taxesSS`; CA-only NOL suspension & DE-4 hidden for
      other states; no-tax states say "no state income tax").
- [x] Math unchanged: self-test all pass (45 cases, 0 fail); no console errors; CA byte-identical.
- [x] Deployed (git push → Vercel) and live (estimated-taxes.vercel.app = 200, new markers served).
**Files involved:** `index.html` (new `updateStateLabels()` + span/id hooks + summary signatures).
**Status:** COMPLETE & deployed (commit `1099d91`).

## Next request goes here (overwrite this block when you start real work)
**Request:** _(describe the next change)_
**Acceptance criteria:** _(bullets)_
**Files likely involved:** _(rg/search first; the whole tool is index.html)_
**Status:** not started
