# BNG Outreach Pipeline · Dashboard

Single-page static dashboard for the cold-outreach pipeline pilot. Shows the Zoho CRM snapshot, Apollo dedup result, the Ready / Clay-enrichable / Reject categorization, and 10 sample drafted emails.

## Local preview

```bash
# Any static file server works. From this folder:
python3 -m http.server 8080
# then open http://localhost:8080
```

Or just double-click `index.html` &mdash; everything is inline (Tailwind via CDN, data embedded as JSON in `<script type="application/json">` blocks).

## Navigation

The dashboard uses **hash-based routing**, not anchor scrolling. Each nav button swaps the visible page rather than jumping to a section. URL bar reflects the current view:

- `#/`            — Overview (default)
- `#/flow`        — Pipeline flow
- `#/zoho`        — Zoho CRM snapshot
- `#/apollo`      — Apollo dedup funnel
- `#/categorize`  — Ready / Clay / Reject categorization
- `#/emails`      — 10 pilot drafts
- `#/rerun`       — Re-run instructions, skill, scripts, send pacing

Every page also has a Previous / Next pager at the bottom for sequential review. Browser back/forward works naturally.

## Deploy to GitHub Pages

```bash
cd /home/bngsys/Projects/enrichment/dashboard
git init -b main
git add .
git commit -m "Outreach pipeline pilot dashboard"
git remote add origin git@github.com:<owner>/<repo>.git
git push -u origin main
```

Then in the repo settings → **Pages** → Source = `Deploy from a branch` → Branch = `main` / `/ (root)` → Save.

The site will be available at `https://<owner>.github.io/<repo>/` within ~1 minute.

## Files

| File | Purpose |
|---|---|
| `index.html` | Whole dashboard. Sections: Hero · Flow · Zoho · Apollo · Categorize · Emails · Re-run · Footer |
| `.nojekyll` | Tells GitHub Pages to skip Jekyll processing |
| `README.md` | This file |

## Updating the data

All numbers and email content are inlined inside three `<script type="application/json">` blocks near the bottom of `index.html`:
- `#zoho-fill-data` &mdash; the Zoho field-fill rate bars
- `#zoho-flow-data` &mdash; the Flow A/B/C bucket sizing
- `#emails-data` &mdash; the 10 sample emails

To refresh after a new run of the pipeline, regenerate the source files via the `/outreach-batch` skill and copy the new numbers into those three blocks.

## Source data

Generated from these files (not included in the dashboard repo):

- `analysis/2026-05-08-1545/analysis.md` &mdash; Zoho CRM snapshot
- `analysis/dedup-apollo-2026-05-08-1555/summary.json` &mdash; Apollo dedup
- `analysis/categorized-2026-05-08-1654/categorization_summary.json` &mdash; Ready/Clay/Reject split
- `analysis/pilot-emails-2026-05-08/drafts.json` &mdash; the 10 pilot emails
- `analysis/usaindiacfo-facts/facts.json` &mdash; verified USAIndiaCFO facts
