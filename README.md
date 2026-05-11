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

- `#/`         — Overview (default)
- `#/funnel`   — Data funnel: dedup → tier scoring → bulk draft → rejects (donut + stacked bars + reject reasons)
- `#/audience` — Who's in the list: CRM snapshot + Apollo audience shape (pies for industry + title)
- `#/drafts`   — Output quality: word-count histogram, audit results, 10 sample emails (filterable)
- `#/ops`      — Pipeline flow, TAT timeline, send pacing, re-run via `/outreach-batch`

5 nav items total (down from 9). All charts are pure inline SVG — no external libraries.

Every page also has a Previous / Next pager at the bottom for sequential review. Browser back/forward works naturally.

## Deploy to GitHub Pages

Repo name: **`crm-apollo-funnel`**

```bash
cd /home/bngsys/Projects/enrichment/dashboard
git init -b main
git add .
git commit -m "CRM × Apollo outreach funnel dashboard"

# Create the repo on GitHub first (gh CLI shown — or do it via the web UI):
gh repo create crm-apollo-funnel --private --source=. --remote=origin --push

# Or, if creating via web UI:
# git remote add origin git@github.com:<owner>/crm-apollo-funnel.git
# git push -u origin main
```

Then in repo Settings → **Pages** → Source = `Deploy from a branch` → Branch = `main` / `/ (root)` → Save.

The site will be live at `https://<owner>.github.io/crm-apollo-funnel/` within ~1 minute.

> If you keep the repo private, GitHub Pages requires a paid plan. For internal partner review without a paid plan, either make it public (it has no secrets — all data is aggregate counts and public-facing leadership names already on usaindiacfo.com) or use the **Pages → Restrict access to private** option on a paid plan.

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

Generated from the stage-numbered folders in the parent project (not included in the dashboard repo):

- `analysis/01-zoho-snapshot/analysis.md` &mdash; Zoho CRM snapshot
- `analysis/02-apollo-dedup/summary.json` &mdash; Apollo dedup
- `analysis/03-categorized/categorization_summary.json` &mdash; Ready/Clay/Reject split
- `analysis/04-pilot-emails/drafts.json` &mdash; the 10 pilot emails
- `analysis/05-bulk-draft-ready/bulk_draft_summary.json` &mdash; Ready cohort drafts (3,628)
- `analysis/06-bulk-draft-clay/bulk_draft_summary.json` &mdash; Clay cohort drafts (1,273)
- `analysis/usaindiacfo-facts/facts.json` &mdash; verified USAIndiaCFO facts

See `analysis/README.md` for the stage-by-stage layout.
