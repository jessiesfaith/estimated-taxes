# CHANGE_LOG

Newest first. Keep entries to a few bullets - no long logs.

## 2026-06-07 - Initialize agent-state workflow for Estimated_taxes
- **Files:** `CLAUDE.md` + `agent-state/*` (8 files). All ASCII.
- **Reason:** Bring the W-2 Estimated Tax tool into the durable-state workflow (skipped in the bulk
  run because its `.git` was a OneDrive-dehydrated placeholder, mis-read as "not a git repo").
- **Findings:** IS a git repo (branch `main`, 1 commit `642f95e`, clean, no remote). Vercel project
  `estimated-taxes`, CLI-deployed, prod READY + public (estimated-taxes.vercel.app = 200).
- **Verification:** deploy verified via Vercel MCP + live 200; no app code changed.
- **Git:** ran `git init` = harmless no-op reinit on the existing repo. **Deployment:** unchanged.
- **Obsidian:** target recorded (lazy create on first /cleanup-handoff).
