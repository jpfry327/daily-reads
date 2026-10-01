# Daily Reads

A static GitHub Pages site that shows short overviews of new papers. A scheduled Claude task keeps `data/posts.json` up to date. Nothing else needs to change day to day.

## Files

```
index.html        the whole site (HTML, CSS and JS in one file)
data/posts.json   every post currently in the feed
img/              one picture per post: Figure 1 (.webp) or Claude's illustration (.svg), named from the DOI
.nojekyll         tells GitHub Pages to serve files as-is
```

## posts.json

```json
{
  "site": "Daily Reads",
  "updated": "2026-09-30T12:05:00Z",
  "retention_days": 90,
  "featured": ["<id>", "<id>", "<id>"],
  "posts": [
    {
      "id": "10.1101/2026.09.28.123456",
      "date_added": "2026-09-30",
      "published": "2026-09-28",
      "title": "...",
      "authors": "Lastname A, Lastname B, Lastname C, et al.",
      "venue": "bioRxiv",
      "type": "Preprint",
      "doi": "10.1101/2026.09.28.123456",
      "url": "https://doi.org/10.1101/2026.09.28.123456",
      "topics": ["Long-read", "Splicing"],
      "summary": "2-3 sentences: what they did and what they found.",
      "key_points": ["...", "...", "..."],
      "data": "GEO GSE123456",
      "code": "github.com/owner/repo",
      "figure": {
        "kind": "figure",
        "src": "img/10.1101_2026.09.28.123456.webp",
        "caption": "Fig. 1 | One-line description of the figure.",
        "credit": "Lastname et al., CC BY 4.0"
      }
    }
  ]
}
```

- `id`: use the DOI so the same paper is never added twice.
- `date_added`: the day the task added it. The 90-day clock uses this, and the list is sorted by it.
- `type`: `Preprint` or `Article`. Anything from bioRxiv/medRxiv is shown as a preprint.
- `data`, `code`: optional. Leave as `""` if the paper doesn't say.
- `featured`: ids of the 3 posts shown at the top of the front page. Pick them from the last 7 days. If the list is empty or missing, the page shows the three newest posts. Featured posts are left out of the list below them so they don't appear twice.
- `figure`: the post's picture. `kind` is `"figure"` for the paper's own Figure 1 or `"illustration"` for an SVG Claude drew. For an illustration, set `credit` to `"Illustration drawn by Claude from the abstract, not a figure from the paper"`. If `figure` is missing or its file fails to load, the page draws a simple topic picture instead (cell clusters for single-cell, a spot grid for spatial, a sashimi plot for splicing, and so on).
- `example`: only on the sample posts. Delete those posts on the first real run.

## What the scheduled task does each run

1. Find candidate preprints with the **bioRxiv connector** (`mcp__bioRxiv__*` tools), not the raw bioRxiv API:
   - Call `search_preprints` with `date_from` = the date of `updated` and `date_to` = today, once per category in the category list, with `limit=100`.
   - Page with `cursor` (0, 100, 200, ...) until a page returns fewer than 100 results. Don't rely on the `total` field; it can come back as 0.
   - The listing includes revised versions of older preprints, and can list the same DOI more than once. Treat a version above 1 as new only if its DOI has never been posted.
   - `search_preprints` has no keyword search, so read every title and abstract preview and shortlist the relevant ones.
   - Call `get_preprint` on each shortlisted DOI for the full abstract, license and published-journal DOI before deciding.
   - DOIs use either the `10.1101/` or the newer `10.64898/` prefix. Both are valid.
2. Skip any DOI already in `posts.json`.
3. Write `summary`, `key_points`, `topics`, `data` and `code` for each new paper.
4. Pick up to 3 posts from the last 7 days for `featured`.
5. Give every new post a picture:
   - If the paper's license allows reuse (CC BY or CC0, for example; check the `license` field from `get_preprint`) and Figure 1 can be downloaded, save it as `img/<DOI with / replaced by _>.webp`, about 800 px wide and under 150 KB. Set `kind` to `"figure"` and credit the authors and license.
   - Otherwise, draw an SVG illustration and save it as `img/<DOI with / replaced by _>.svg`. Rules for the illustration:
     - `viewBox="0 0 640 400"`, a white background rectangle, and `font-family="Noto Sans, Arial, sans-serif"`.
     - A schematic of the study design or main finding, based only on the title and abstract: cells, tissues, transcripts as exon boxes, arrows between steps, simple bar or dot charts.
     - Use these colours: blue #1E5FA8, green #3C9A2E, red #C8323F, orange #D98A00, grey #8A949E, dark text #2B3138.
     - A short title line at the top and labels of four words or fewer. Never copy the paper's numbers into a chart unless the abstract states them, and never imitate a published figure.
     - Plain shapes only: no scripts, no external images or fonts, under 20 KB.
     - Set `kind` to `"illustration"` and use the standard credit line above.
6. Remove posts whose `date_added` is more than `retention_days` ago.
   Delete their image files from `img/` too.
7. Set `updated` to now, commit, and push.

The page also hides posts older than `retention_days`, so the feed stays correct even if a cleanup run is missed.

## Saved posts

Saving stores a full copy of the post in the browser's local storage, so saved posts survive the 90-day cleanup. Saves are per browser and do not sync between devices. A saved post whose figure file has been deleted shows the simple topic picture instead.
