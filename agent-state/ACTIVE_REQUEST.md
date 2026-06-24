# ACTIVE_REQUEST

_Last updated: 2026-06-24_

## Current active request
Clean & handoff (after the LinkedIn-launch work: state-name labels, Field Guide, privacy statement, live reminders).

## Acceptance criteria
- [x] Code scanned for dead/leftover — grep clean + 2-agent adversarial scan; removed the one finding (unused `.source-head` CSS).
- [x] Wiring sanity-check of all 4 session features = no issues.
- [x] Self-test all pass (45); no console errors; prod 200.
- [x] All agent-state docs refreshed to current truth (git-push deploy; all 50 states; field guide; privacy; reminders live).
- [x] Working tree committed & pushed.

## Status
- **Overall: COMPLETE — clean & handed off (2026-06-24).** Nothing mid-flight.

## Session summary (2026-06-23/24)
1. State-name labels follow the selected state everywhere (commit `1099d91`).
2. In-app Field Guide — right pane, 75 fields, broad/example/detail + focus-sync (`98ee02c`).
3. Explicit privacy statement — banner + footer disclaimer + header tag (`88fb1d3`).
4. Reminders signup wired LIVE to Formspree -> info@fastinsights.io + SMS/marketing opt-ins + analytics note (`7a42acd`). Capture only; automated SENDING still TODO.
5. Clean & handoff: removed dead `.source-head` CSS; refreshed all agent-state docs.
Plus LinkedIn launch assets (post copy, 30s video MP4, brand banners, Gantt/Tracker/Template/Checklist workbook) in `00 Command Center`.

## Next request goes here (overwrite this block when you start real work)
**Request:** _(describe the next change)_
**Acceptance criteria:** _(bullets)_
**Files likely involved:** _(the whole tool is index.html)_
**Status:** not started
