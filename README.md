# Business internship tracker (marketing / sales / consulting / BD)

Checks the careers sites of about 75 consulting firms, ad agencies, brands, media and tech companies,
and sends a phone alert when a new **marketing, sales, consulting or business development** internship
opens in **NYC or Boston** that a **sophomore (class of 2029)** can apply to.
It runs free on GitHub Actions.

This is the sister of the AM / ER / credit tracker. It uses the same code, but its own settings
(`filters.yaml`, `firms.yaml`) and its own ntfy topic.

## What gets alerted

| Priority | What | Alert |
|---|---|---|
| Top | Marketing, brand, advertising, sales, account management, consulting, business development, partnerships. At consulting firms and ad agencies, any non-tech/finance internship. | Instant push. Tapping it opens the job. |
| Related | Communications, PR, creative, editorial, audience, merchandising, client services | One grouped push per run |
| Low | Tech, finance, ops, R&D, HR, recruiting events | Dashboard only |
| Ignored | Not an internship, MBA / PhD / Masters level, wrong summer, not in NYC or Boston | Nothing |

**Class year.** Every **summer 2028** internship (after junior year) counts. **Summer 2027** internships count
only if the posting says sophomores, rising juniors or the class of 2029 can apply. Undated postings are kept
unless they name a different class year. The tracker reads each posting's description once to check.

**Location.** Only NYC (Manhattan, Brooklyn) and Boston / Cambridge, MA. Postings with a vague location
("2 Locations", "United States", "Remote", or none) are kept only if the description mentions NYC or Boston.

The dashboard (`JOBS.md`) shows a **Who can apply** reason for every posting.

## Setup

1. **ntfy app:** subscribe to this tracker's topic (a different one from the AM tracker).
2. **GitHub:** create a new **public** repo and upload every file in this folder. The hidden `.github`
   folder won't upload by drag-and-drop, so create `.github/workflows/tracker.yml` with **Add file → Create new file**
   and paste its contents.
3. **Secret:** Settings → Secrets and variables → Actions → New repository secret, named `NTFY_TOPIC`,
   containing your topic name.
4. **Actions** tab → **business-internship-tracker** → **Run workflow**. First tick "Just send a test notification",
   then run it again with nothing ticked to get the "tracker is live" summary.

The optional Google Sheet works the same as in the AM tracker (`sheets/apps_script.gs`).

## Customizing

- **Keywords:** the lists in `filters.yaml`.
- **Cities:** `only_locations` in `filters.yaml`.
- **Companies:** `firms.yaml`. The comments at the top show how to add one from its "Apply" link.
  `kind: focus` means any client-facing internship there counts (used for consulting firms and agencies).
- **Check by hand:** the `manual:` list at the bottom of `firms.yaml` (McKinsey, Bain, Deloitte, L'Oréal,
  Google…). These block automated checks, so join their campus mailing lists.

## Limits

- Big names on the manual list can't be checked automatically.
- GitHub's scheduler runs it roughly every few hours, not every 30 minutes.
- If a company's site changes, you get a "Tracker can't check X" push after 3 failed runs.
