Continue the Estimated_taxes (Estimated Tax & Safe-Harbor Tool) from durable state, not the old chat.

First read only:
- CLAUDE.md
- agent-state/HANDOFF.md
- agent-state/CURRENT_STATE.md
- agent-state/ACTIVE_REQUEST.md
- agent-state/DEPLOYMENT_STATUS.md
- agent-state/VERIFICATION_STATUS.md
- agent-state/CHANGE_LOG.md
- git status / git log -1 --oneline

Do not scan the whole repo. The whole tool is index.html.
Run from inside: 01 Projects/Fast Insights/10 Code Repos/Estimated_taxes

Current state:
Federal + all 50 states + DC estimated-tax & safe-harbor tool. Shipped this session: state-name labels follow the
selected state; in-app Field Guide (right pane, 75 fields); explicit privacy statement; LIVE reminders signup
(Formspree -> info@fastinsights.io, capture only). All deployed & live. Clean & handed off 2026-06-24.

Active request status:
COMPLETE & deployed; clean & handed off. Nothing mid-flight.

Local commands:
- dev: npx http-server . -p 8124 -> http://localhost:8124
- test: in-app "Run self-test" (45 cases, must all pass)
- deploy: git push origin main -> Vercel auto-deploys

Verification:
- self-test all pass (45); no console errors; prod 200 (estimated-taxes.vercel.app + app.fastinsights.io/estimated-taxes)

Deployment:
git-connected Vercel; push to main auto-deploys. Remote origin = git@github-jessica:jessiesfaith/estimated-taxes.git.

PRIVACY: never commit/deploy personal docs (paystubs/PDFs are git+vercel-ignored; .vercelignore ships only index.html).

Likely next actions:
- Automated reminder SENDER (capture is live; sending is not) -> cron + email/SMS. Email path for the Fast Insights app
  is Supabase Edge Function + Resend; SMS would be Twilio. (See memory: fast-insights-supabase-email.)
- Optional polish: refresh CA exemption credit to 153/306; reseed a_ca_exempt in clearAll(); add Open Graph meta tags
  (LinkedIn unfurl); enable Vercel Web Analytics (backs the analytics note); tighten any medium-confidence state.

Return: what you read | repo state | active request status | deployment status | blockers | first small action.
