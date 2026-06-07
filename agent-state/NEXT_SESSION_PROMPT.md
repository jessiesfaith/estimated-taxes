Continue the Estimated_taxes (W-2 Estimated Tax & Safe-Harbor Tool) repo from durable state, not the old chat.

First read only:
- CLAUDE.md
- agent-state/HANDOFF.md
- agent-state/CURRENT_STATE.md
- agent-state/ACTIVE_REQUEST.md
- agent-state/DEPLOYMENT_STATUS.md
- agent-state/VERIFICATION_STATUS.md
- agent-state/OBSIDIAN_SYNC.md
- git status / git log -1 --oneline

Do not scan the whole repo. Do not rely on the prior chat. The whole tool is index.html.
Run from inside: 01 Projects/Fast Insights/10 Code Repos/Estimated_taxes

Current objective:
Workflow setup complete. Awaiting the next real change to the tool.

Active request status:
complete (workflow setup). No app code change in progress.

Local commands:
- dev: python -m http.server 8000  -> http://localhost:8000
- build/test/lint/typecheck: none (static)
- deploy: npx vercel --prod  (Vercel CLI; NO git auto-deploy)

Verification status:
- build/test/lint/typecheck: N/A
- localhost: not running
- deployment: VERIFIED READY + public - estimated-taxes.vercel.app = 200 (Vercel MCP + live check)

Deployment:
Vercel CLI (project estimated-taxes), prod READY + public. Manual deploy via npx vercel --prod. No git remote/auto-deploy.

Obsidian:
Vault = C:\Users\dough\OneDrive\_Jessica\Builder_OS. Target "01 Projects/Fast Insights/00 Command Center/Agent State/Estimated_taxes/". Update only for meaningful changes.

Uncommitted changes:
agent-state/ and CLAUDE.md are untracked (new). index.html unchanged. Git: branch main, 1 commit 642f95e, no remote.

Next action:
Use /delta-update <change>. PRIVACY: never commit or deploy personal docs (paystubs/PDFs are git+vercel-ignored). After edits, deploy with npx vercel --prod.

Return: what you read | repo state | active request status | deployment status | blockers | first small action.
