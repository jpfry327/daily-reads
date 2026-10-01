# Daily Reads

A static GitHub Pages site that shows short overviews of new papers. A scheduled Claude task keeps `data/posts.json` up to date. Nothing else needs to change day to day.

## Files

```
index.html        the whole site (HTML, CSS and JS in one file)
data/posts.json   every post currently in the feed
data/updates.json the task's report from each of its last 7 runs, shown in the Update tab
config/interests.md  what to post: topics, tags, journal tiers, preprint rules, limits
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
      "tier": null,
      "doi": "10.1101/2026.09.28.123456",
      "url": "https://doi.org/10.1101/2026.09.28.123456",
      "topics": ["Methods", "Long-read", "Splicing"],
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
- `tier`: `1`, `2` or `"review"` for journal papers, from the journal lists in `config/interests.md`. `null` for preprints and for chordoma papers from other journals.
- `topics`: the main topic first (`Chordoma`, `Cancer genomics`, `RNA biology` or `Methods`), then up to 3 tags. Purely computational papers always include the `Computational` tag. Only use names listed in `config/interests.md`.
- `data`, `code`: optional. Leave as `""` if the paper doesn't say.
- `featured`: ids of the 3 posts shown at the top of the front page. Pick them from the journal papers added in this run, Tier 1 first, never preprints or chordoma posts. If this run added fewer than 3, keep the most recent earlier featured posts to fill the rest. Only featured posts get a picture (step 7). If the list has fewer than 3 usable ids, the page fills the rest with the newest journal papers. Featured posts are left out of the list below them so they don't appear twice.

## What the page shows

- **Home**: 3 featured journal papers, then the list. Journal papers only, and no chordoma posts. Preprints appear only when the reader picks "Preprints" (or "Both") under "Show".
- **Chordoma** tab: every chordoma post, journal papers and preprints alike.
- **Update** tab: the reports from `data/updates.json`, newest first.
- **Saved** tab: everything the reader saved.
- `figure`: the post's picture. Only featured posts have one; leave it out for every other post. `kind` is `"figure"` for the paper's own Figure 1 or `"illustration"` for an SVG Claude drew. For an illustration, set `credit` to `"Illustration drawn by Claude from the abstract, not a figure from the paper"`. In the list, a post without a `figure` (or whose file fails to load) shows no thumbnail. In the featured row, the page draws a simple topic picture instead.
- `example`: only on the sample posts. Delete those posts on the first real run.

## What the scheduled task does each run

Read `config/interests.md` first. It decides what counts and how many to post.

1. Look back **7 days** (from 7 days before today up to today), not just since the last run.
2. Find journal papers with the **PubMed connector** (`mcp__PubMed__*` tools):
   - Run the searches listed in `config/interests.md`: one per main topic, limited to the Tier 1 and Tier 2 journals with `[ta]`, the review journals with `Review[pt]`, the Tier 1 computational search, and `chordoma` with no journal limit.
   - Use `datetype: "edat"` (the date PubMed added the paper) and page with `retstart` until every result has been read.
   - Get metadata (abstract, DOI, journal) with `get_article_metadata`, and check reuse with `get_copyright_status`.
3. Find preprints with the **bioRxiv connector** (`mcp__bioRxiv__*` tools), not the raw bioRxiv API:
   - Call `search_preprints` with the 7-day `date_from`/`date_to`, once per category listed in `config/interests.md`, with `limit=100`.
   - Page with `cursor` (0, 100, 200, ...) until a page returns fewer than 100 results. Don't rely on the `total` field; it can come back as 0.
   - The listing includes revised versions of older preprints, and can list the same DOI more than once. Treat a version above 1 as new only if its DOI has never been posted.
   - `search_preprints` has no keyword search, so read every title and abstract preview, including for chordoma, and shortlist the relevant ones.
   - Call `get_preprint` on each shortlisted DOI for the full abstract, license and published-journal DOI before deciding.
   - DOIs use either the `10.1101/` or the newer `10.64898/` prefix. Both are valid.
   - If a preprint has since been published in a journal, post the journal version instead (if it qualifies) and skip the preprint.
4. Skip any DOI already in `posts.json`. Apply the rules and limits in `config/interests.md`: tier rules for journals, close-match rule for preprints, at most 10 journal papers and 10 preprints per run, no limit on chordoma. Zero is fine.
5. Write `summary`, `key_points`, `topics`, `tier`, `data` and `code` for each new paper.
6. Pick 3 journal papers from the last 7 days for `featured` (see above).
7. Give a picture to **the 3 featured posts only**. No other post gets a `figure`. The page shows posts without one with no thumbnail, so over time a picture marks a paper that was featured. A post that already has a picture from an earlier run keeps it.
   - If the paper's license allows reuse (CC BY or CC0; check `get_copyright_status` for PubMed papers or the `license` field from `get_preprint`) and Figure 1 can be downloaded, save it as `img/<DOI with / replaced by _>.webp`, about 800 px wide and under 150 KB. Set `kind` to `"figure"` and credit the authors and license.
   - Otherwise draw an SVG illustration and save it as `img/<DOI with / replaced by _>.svg`. With only 3 per run, take care over each one:
     - Draw each one by hand for that paper. Never use a script, template or shared function that generates several illustrations.
     - Show this paper's specific mechanism or result, using the actual named molecules, cells, variants or steps from the abstract. For example: a break at a TA repeat on an ecDNA circle, repaired by MMEJ, versus ecDNA lost under Pol θ inhibition. A row of generic boxes ("data → model → result") or a decorative motif (dot clusters, networks, blobs) is not enough on its own.
     - Use a different composition from the other illustrations in the feed. Look at the existing `img/*.svg` before drawing.
     - Never imply numbers the abstract doesn't state: no bar heights or curve values that suggest a size of effect. Use schematic shapes, a Venn diagram, or a labelled before/after instead.
     - `viewBox="0 0 640 400"`, a white background rectangle, and `font-family="Noto Sans, Arial, sans-serif"`. Text at least 13 px.
     - Use these colours: blue #1E5FA8, green #3C9A2E, red #C8323F, orange #D98A00, grey #8A949E, dark text #2B3138.
     - A short title line at the top and labels of four words or fewer. Never imitate a published figure.
     - Plain shapes only: no scripts, no external images or fonts, under 20 KB.
     - Render it (e.g. with Playwright) and look at it. Fix overlapping text or shapes before committing.
     - Set `kind` to `"illustration"`, write a one-sentence `caption` describing what it shows (not "Schematic of the study"), and use the standard credit line.
8. Remove posts whose `date_added` is more than `retention_days` ago.
   Delete their image files from `img/` too.
9. Check `data/posts.json` before committing: it must be valid JSON, every post needs `id`, `date_added`, `title`, `venue`, `type` and `topics`, topics must come from `config/interests.md`, and every `figure.src` file must exist. If any check fails, fix it, and don't push a broken file.
10. Add this run's report to the front of `updates` in `data/updates.json`, and keep only the 7 newest. Delete any report with `"example": true`. Write a report even when nothing was added. Format:
    ```json
    {
      "date": "2026-10-02T10:05:00Z",
      "summary": "One or two sentences on the run.",
      "counts": { "journal": 0, "preprint": 0, "chordoma": 0, "removed": 0 },
      "added": [{ "id": "<post id>", "title": "...", "venue": "...", "tier": 1, "topic": "<main topic>" }],
      "skipped": [{ "title": "...", "reason": "one line" }],
      "notes": ["anything unclear in the rules, missing tools, failed downloads"]
    }
    ```
    `skipped` holds at most 10 of the closest misses. `counts.chordoma` counts chordoma posts separately; they are not also counted under `journal` or `preprint`. `removed` is the number of posts the 90-day cleanup deleted.
11. Set `updated` in `posts.json` to now, check that `updates.json` is valid JSON, commit, and push.

The page also hides posts older than `retention_days`, so the feed stays correct even if a cleanup run is missed.

## Saved posts

Saving stores a full copy of the post in the browser's local storage, so saved posts survive the 90-day cleanup. Saves are per browser and do not sync between devices. A saved post whose figure file has been deleted shows the simple topic picture instead.
