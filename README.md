# LA Concept Art Job Search

A static site tracking verified-live environment/character concept art, visual development, and 2D design openings in Los Angeles games and animation — plus a networking map of studio art leadership.

**Live site:** https://USERNAME.github.io/REPO/ *(update after enabling Pages)*

Verified August 8, 2026.

## Pages

| File | What it is |
|---|---|
| `index.html` | Landing page — headline numbers, market context, union guidance |
| `jobs.html` | Filterable tracker of every verified-live role, with per-role application status |
| `network.html` | 60 pre-filtered LinkedIn 2nd-degree searches + named art leads per studio + LA events |

All three are self-contained: inline CSS and JS, no build step, no dependencies, no external requests except outbound links.

## Deploying

Pages serves straight from the `main` branch root — no build step, no workflow file. After the first push:

**Settings** → **Pages** → **Source: Deploy from a branch** → `main` / `/ (root)`

Or via the CLI:

```
gh api -X POST repos/:owner/la-art-jobs/pages -f "source[branch]=main" -f "source[path]=/"
```

Every subsequent `git push` redeploys automatically. `.nojekyll` is present so Jekyll doesn't touch the files.

See `SETUP.md` for the full first-time walkthrough.

## Editing the job list

All job data lives in one array at the bottom of `jobs.html`:

```js
const JOBS = [
  {studio:"...", title:"...", ind:"games"|"animation", loc:"la"|"remote",
   locText:"...", type:"staff"|"contract", typeText:"...", exp:"...",
   pay:"...", focus:"character"|"environment"|"both",
   fit:"fit"|"stretch"|"reach", note:"...", warn:"...", url:"...",
   group:"top"|"second"|"remote"},
];
```

Add, remove, or edit entries there — the stat tiles, filters and grouping all derive from the array, so nothing else needs touching. `warn` is optional; omit it and no warning line renders.

Networking contacts in `network.html` are plain HTML tables — edit directly.

## Verification standard

A role appears in `jobs.html` only if its own job board loaded **and** an application form rendered. Postings that merely appear in search results were excluded and listed separately under the dead-postings section — that distinction matters more than expected here. Several widely-indexed listings (Naughty Dog's Senior Concept Artist, seven PlayStation-hosted concept roles, six Blizzard requisitions) are closed but still rank well in search.

Named individuals come from public professional sources only: studio rosters, published credits, ArtStation Art Blasts, conference programs, trade press. No private contact information is included. ArtStation Art Blast rosters are point-in-time snapshots of shipped projects — confirm current employer before referencing anyone.

## A note on visibility

This repo is public, so the site is indexable. Everything on it is already public professional information, but if you'd rather it not surface in searches for the people named in `network.html`, add this to each page's `<head>`:

```html
<meta name="robots" content="noindex, nofollow">
```

Or make the repo private (GitHub Pages on private repos requires a paid plan).

## Refreshing

Job postings go stale fast — assume a useful life of about four weeks. When re-checking, the fastest path is to open each `url` in the `JOBS` array and confirm the application form still renders, then update or remove entries.
