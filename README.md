# CBSM guide

An unofficial quick-reference site for Minnesota DHS's Community-Based Services Manual (CBSM), built for waiver case managers. It's live at https://ramsey-cm-guide.github.io/CBSM/.

## What the site has

- **CBSM table of contents.** Links to all CBSM pages. They open the official DHS pages, because DHS's site blocks automated copying.
- **Forms.** The DHS forms case managers complete, gather or give out, with what each is for and a link to the current version on eDocs. "All forms" adds DHS's frequently used forms list.
- **Bulletins.** Ramsey County ADS Case Management Bulletins, collected daily.
- **Search.** Covers CBSM page titles, forms and bulletins. Acronyms like CADI, EW and PCA are expanded.
- **What changed.** New bulletins and revised forms, checked daily.
- **Rules feed.** `site/data/rules.csv` lists the CBSM-sourced numbers the Excel tracker's Rules tab checks itself against (day counts, dollar limits, dates).

## What's in here

```
scraper/scrape.py            daily sync (link mode by default)
scraper/cbsm_toc.json        the CBSM table of contents the site links to
scraper/bulletins.py         collects Ramsey County ADS bulletins
scraper/bulletins.txt        bulletin links to add by hand, one per line
scraper/forms.py             builds the forms list and checks for revisions
scraper/forms_catalog.json   case manager forms, with descriptions (edit freely)
scraper/forms_dhs_list.txt   DHS's frequently used forms list
scraper/forms.txt            extra form numbers to include, one per line
scraper/requirements.txt     Python packages the sync needs
site/index.html              the website
site/data/                   written by the sync; the site reads from here
site/data/rules.csv          edited by hand, not by the sync (see below)
.github/workflows/sync.yml   runs the sync daily and publishes the site
```

## Day to day

Nothing. The sync runs every morning at about 5 AM Central. To run it now, go to Actions, choose "Sync CBSM and deploy," and click "Run workflow."

## Common edits

- **Add a bulletin the sync missed:** open `scraper/bulletins.txt`, paste the bulletin's "View as a webpage" link on a new line, and commit.
- **Add a form:** add its number (and optionally its title) on a new line in `scraper/forms.txt`.
- **Change a form's description:** edit `scraper/forms_catalog.json`.

## Updating the rules feed

When DHS changes a number the tracker uses, edit the matching row in `site/data/rules.csv` (ID, Value, Unit, Effective from, Last verified, Source) and commit. Committing the file redeploys the site. Trackers show the new number in the Rules tab's "Site value" column and flag it CHANGED until each case manager accepts it. Only CBSM-sourced rules go in this file; agency practice stays in the tracker.

## Things to know

- **This is unofficial.** DHS's site is the authority on policy. CBSM links always open the official pages.
- **Policy page changes aren't tracked.** DHS's site shows a bot check (CAPTCHA) to automated requests, so the sync doesn't download CBSM pages. Subscribe to DHS's DSD eLists for manual updates.
- **If GovDelivery or eDocs block automated requests too,** the sync still succeeds and the log says so; the site keeps the links.
- **The Excel tracker** reads only `site/data/changes.json` (new bulletins and revised forms) and `site/data/rules.csv`. Nothing from the tracker is ever sent here.
