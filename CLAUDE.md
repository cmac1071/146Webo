# CLAUDE.md — Pool & Basement Project

## Project overview
Static HTML website for the pool and basement construction project at 146 Westboro Rd, Upton MA.
Hosted on GitHub Pages: https://cmac1071.github.io/146Webo/
Repo: git@github.com:cmac1071/146Webo.git (branch: main)
Local path: /Users/Chris/Documents/146 Westboro Rd/pool/

## Audience and content rules
The site is shared with family and friends. **Never add costs, dollar amounts, payment milestones, budget information, or contract figures of any kind.** No Google Drive links. No historical timelines or deadlines.

Keep: contractor comparisons (no pricing), pool type analysis, insurance checklist, permitting info, photos.

## Site structure
- index.html — project overview
- contractors.html — contractor comparison (no costs)
- pool-comparison.html — pool type analysis (titled "Pool Types" in nav)
- insurance.html — homeowner insurance checklist
- permitting.html — Upton MA permitting status
- photos.html — construction photo log
- budget.html — DELETED; do not recreate
- style.css — shared stylesheet

Nav order: Overview · Contractors · Pool Types · Insurance · Permitting · Photos

## Photos convention (photos.html) — restructured 2026-08-06

### Layout
Section order, top to bottom:
1. **Plans & Sketches** — Pool-patio-fence-sketch-1.jpg. Its own thing: uses `openLightboxDirect()`, has its own `.photo-caption` (title + body text) inline in the card, no lightbox nav arrows, NOT in the JS `photos[]` array. Don't touch this section's format.
2. **Before** — Before 1–5.jpeg (indices 0–4). One `<p class="week-description">` under the section-label summarizing the pre-construction yard. Individual photo-cards have NO caption, just `<img alt="...">`.
3. **Construction, grouped by week** — every group after that is one calendar week of photos: one `<div class="section-label">` + one `<p class="week-description">` (a few sentences of prose summarizing that week's progress) + one `<div class="photo-grid">`. Photo-cards inside have no individual captions or titles — just `<img alt="...">`. Photos are in chronological order within the grid (Sunday's before Monday's, etc.).

### Week boundary rule
Weeks run **Sunday → Saturday**. The first-ever group (June 27, 2026) is a one-time historical exception: it started truncated, on the actual date of the first construction photo (a Saturday), instead of the preceding Sunday. Every week since follows the normal Sun–Sat calendar boundary.

**Section-label text = the actual span of dates that have photos that week**, not the full Sun–Sat range — e.g. a week with photos only on Tue/Wed/Thu is labeled `August 4–6, 2026`, not "Week of August 2–8". Single-day weeks get just that date (`June 27, 2026`). Use `&ndash;` for the dash. No title/subtitle after the date — just the date span.

### JS structure
Each week's description is defined once as a `const WEEK_XXX = '...'` near the top of the `<script>` block (e.g. `WEEK_JUL6_10`, `WEEK_AUG4_6`). Every entry in the `photos[]` array for photos in that week points its `caption` field at that same shared constant — so every photo in a week shows the identical text in the lightbox (the week's description), not a unique per-photo caption. `photos[]` is still one flat array in page order; the index numbering is a running count across the whole page, unaffected by week grouping. The Before section uses the same pattern (`WEEK_BEFORE`). The sketch is not in this array.

### Adding new photos — procedure
1. `ls Photos/` to find files not yet in photos.html.
2. Resize before committing — iPhone photos come in at 7–8MB and can break GitHub Pages deployment. `sips -Z 2000 <file>` on Mac (or PIL `img.thumbnail((2000,2000))`), targets max 2000px long edge, ~under 1MB.
3. View each new photo to understand what it shows.
4. Determine which week group it belongs in by date, using the Sun–Sat rule:
   - **Same Sun–Sat week as the current last group on the page:** add to that existing group. Append the photo-card to the end of that `photo-grid` in the correct chronological spot. Update the section-label's date span if it now extends further (e.g. "August 4–6" → "August 4–7"), and revise both the `week-description` paragraph AND its matching `WEEK_XXX` JS constant to fold in what the new photo(s) show — don't leave the old description stale.
   - **New week beyond the current last group:** create a new section (`section-label` + `week-description` + `photo-grid`) and a new `WEEK_XXX` constant.
5. Give each `<img>` a plain, accurate `alt` attribute for accessibility only — not a visible caption. No `.photo-caption` div per photo.
6. **Don't name a camera vantage point** (deck / driveway / security camera / upstairs window / etc.) in any caption or description — Chris's instruction as of 2026-07-25. A viewer can infer vantage from context across the page. Just describe what's happening/visible.
7. Append entries to the `photos[]` array (`{ src: 'Photos/X.jpeg', caption: WEEK_XXX }`), continuing the index numbering from the last one used.
8. Commit resized photos + updated photos.html together.
9. **Do not push.** Chris owns the git push — after committing, tell him it's ready and he should run `git push origin main` himself.

### Video clips
The page can embed video the same way as photos. Example: `Photos/PoolPlacement.mp4` (20-sec, 100x-speed Ring security camera clip of the pool shell being placed, shot July 15, 2026), sitting inside the July 13–15 week's `photo-grid` as a `.photo-card.wide.video-card` containing a native `<video controls preload="metadata" poster="...">` — no `data-index`/lightbox wiring (the lightbox only swaps an `<img>` src, can't play video), and no individual caption (same as photos). Matching CSS (`.photo-card video` sized like `.photo-card img`) is already in the `<style>` block — reuse it. Videos do NOT go in the JS `photos[]` array and do NOT consume an index number.

**Compress video before committing**, same spirit as the photo resize step:
```
ffmpeg -vf "scale=1280:-2" -c:v libx264 -crf 28 -preset slow -c:a aac -b:a 96k <in> <out>
```
`PoolPlacement.mp4` went from 31MB (1920x1080, ~12.4Mbps) to 4.2MB (1280x720, ~1.5Mbps) with that recipe.

### Naming convention
Construction photos: `MonthDD_N.jpeg` (e.g. `July2_1.jpeg`). Note: August files use abbreviated `Aug` not `August` (e.g. `Aug4_1.jpeg`) — that's how they were uploaded; don't rename to "fix" it.

### Current index state (as of 2026-08-06, commit `f7a3f1b`)
Week groups and their index ranges, oldest to newest:
- 0–4: Before 1–5.jpeg (`WEEK_BEFORE`)
- 5–6: June 27, 2026 — erosion control wattle (`WEEK_JUN27`)
- 7–10: July 2, 2026 — excavation day 1, wall trench + footing (`WEEK_JUL2`)
- 11–23: July 6–10, 2026 — wall blocks delivered, pool shell arrives, basin excavated, wall reaches 3 courses (`WEEK_JUL6_10`)
- 24–28: July 13–15, 2026 — wall finished/corner turned, shell lowered into place (+ `PoolPlacement.mp4`, no index) (`WEEK_JUL13_15`)
- 29–37: July 19–22, 2026 — shell backfilled, plumbing roughed in, wall built to top course (not yet capped) (`WEEK_JUL19_22`)
- 38–45: July 30–31, 2026 — wall finally capped, filter/heat pump installed, **pool swimmable as of July 31** (`WEEK_JUL30_31`)
- 46–52: August 4–6, 2026 (current/most-recent group as of this writing) — footprint expanding beyond the wall for patio prep (`WEEK_AUG4_6`)

**Next new photos start at index 53.** If dated Aug 2–8, 2026, add to the existing "August 4–6" group per the rule above (relabeling the date span as needed); if later, start a new week group.

There was a construction pause between July 22 and July 30, 2026 (reasons not specified — not for the site).

## Git workflow
- Chris controls all git pushes. Always stop at `git commit` and tell him the commit is ready.
- Never attempt `git push`.
