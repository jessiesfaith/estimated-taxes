# Estimated Taxes Tool

Single self-contained `index.html` — a W-2 **estimated-tax & safe-harbor calculator** (Fast Insights).
It reads paystubs / W-2s / 1099s locally in the browser (nothing is uploaded), projects the full
year, and shows the federal + California estimate, the 90 / 100 / 110% safe-harbor targets, the
per-paycheck withholding adjustment, and the quarterly estimated-payment schedule.

## Run locally

    python -m http.server 8000

Then open http://localhost:8000

## Deploy

Vercel project `estimated-taxes`, served at https://app.fastinsights.io/estimated-taxes/ :

    npx vercel deploy --prod --yes

The deploy ships only `index.html` (see `.vercelignore`); personal documents are never uploaded.

---

Estimate only — not tax or legal advice.
