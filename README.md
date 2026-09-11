# State Flow Mapper

Visualize how a categorical field changes over time — built to compare change
logs from one or more raters/annotators side by side (arc, circular,
force-directed, timeline, and Sankey pass-flow layouts).

Live site: https://lensprague.com/stateflowviz/
Examples: https://lensprague.com/stateflowviz/examples/

## Quick start

1. **Install Hugo Extended** — Windows: `winget install Hugo.Hugo.Extended` (or [download](https://github.com/gohugoio/hugo/releases))
2. **Open a terminal** in the project folder and run:
   ```
   hugo server
   ```
3. **Open in browser:** http://localhost:1313/

**Useful links:**
- **Full editor** (upload your own CSVs): http://localhost:1313/
- **Examples:** http://localhost:1313/examples/

## How it works

- Drop one or more CSV files onto the homepage (`rowId, rowIndex, field, oldValue, newValue, timestamp` columns). Multiple files can be compared via the log picker, including an "All logs combined" view (merge by real timestamp or by each rater's own step order).
- The `/examples/` section is a small Hugo-generated gallery. Each example page is the same tool, pre-loaded with a fixed set of CSVs declared in that example's front matter (`content/examples/*.md`, `dataset_urls` param) and pointing at files under `static/data/`.
- The interactive app itself (HTML/CSS/JS) lives in one place, `layouts/partials/app.html`, and is reused by both the homepage (`layouts/index.html`) and every example page (`layouts/_default/example-viz.html`) — see the "single shared partial" note below if you're extending this pattern elsewhere.

## Repo layout

```
hugo.toml                          site config (baseURL, title)
layouts/
  index.html                       homepage wrapper (head boilerplate + partial)
  _default/example-viz.html        example-page wrapper (head boilerplate + partial)
  examples/list.html               /examples/ gallery page
  partials/app.html                the actual app: styles, markup, and JS (single source of truth)
content/
  examples/_index.md               /examples/ gallery title+description
  examples/all-raters.md           the combined-comparison example's front matter + dataset_urls
static/
  data/*.csv                       example CSVs served as static files
_archive/
  state-flow-mapper-v6.html        original single-file version, kept for reference
```

## Adding another example

1. Drop the CSV(s) into `static/data/`.
2. Add a `content/examples/<slug>.md` file:
   ```toml
   +++
   title = "My Example"
   description = "One sentence describing what it shows."
   dataset_urls = ["data/my-file.csv"]
   layout = "example-viz"
   date = 2026-09-07
   +++
   ```
3. That's it — `layouts/_default/example-viz.html` + `layouts/partials/app.html` handle the rest; the new example appears automatically on `/examples/`.

## Deployment

Pushes to `main` build and deploy via `.github/workflows/hugo.yml` to GitHub Pages (Pages must be set to source **GitHub Actions** in the repo settings).

## Troubleshooting

| Problem | What to try |
|--------|-------------|
| "hugo is not recognized" | Install Hugo and open a **new** terminal. |
| Page won't load locally | Make sure the terminal running `hugo server` is still open. |
| Port 1313 in use | Run `hugo server --port 1314` and use http://localhost:1314/ instead. |
