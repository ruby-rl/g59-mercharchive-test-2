# G59 Merch Archive — GitHub Pages version

Real folders with real photos, and the pages build themselves from
whatever's in those folders. You never hand-edit HTML to add a drop — you
just make a folder, drop photos in, and push.

## How it's organized

Category folders sit right at the top of the repo:

```
drop/
  2024-10-18-fw-essentials/
    info.json      <- optional: title, shortTitle, date, link
    banner.jpg      <- optional: low-opacity banner behind the title
    preview.jpg     <- optional: cover photo shown big at the top
    IMG_0231.jpg    <- any other photos, real filenames, any names
    IMG_0232.jpg
tour/
collabs/
samples/
employee/
books/
```

Every folder inside a category becomes one section on that category's
page — heading, optional date/link, a cover photo, and a gallery grid.
`scripts/build.mjs` (run automatically by the GitHub Action on every push)
turns all of this into `drop.html`, `tour.html`, etc. There's nothing to
run yourself.

`index.html` (the home page) is untouched by all of this — it's a plain,
static file, not generated.

---

## The category-page layout

Every category page (drop/tour/collabs/samples/employee/books — not the
home page) shares:

- **A nav bar** across the top linking to every category, with the current
  one underlined and highlighted.
- **A search box** that filters the visible collections by title as you type.
- **A sidebar** listing every collection in the current category — click
  one to jump to it, and whichever section you're scrolled to gets
  highlighted automatically.
- **Infinite scroll** — reach the bottom of one category and the next one
  (in the order set in `data/categories.json`) loads in automatically, so
  you can keep scrolling straight from Drop Merch through to Physical
  Media without clicking anything.

None of this needs any setup per-collection — it all comes for free just
from having folders in place.

---

## Adding a new drop/collection

1. Pick the category folder (`drop/`, `tour/`, `collabs/`, `samples/`,
   `employee/`, or `books/`) at the repo root.
2. Make a new folder inside it, named however you like.
3. Drop your photos straight into that folder. Real filenames are fine.
4. Optional: add an `info.json` file in that folder:
   ```json
   {
     "title": "F/W essentials collection",
     "shortTitle": "F/W 2024",
     "date": "Oct 18, 2024",
     "link": "https://x.com/SUICIDEBOYS/status/1847353128964018189"
   }
   ```
   - `title` — full heading shown at the top of the section (defaults to
     the folder name if you skip this).
   - `shortTitle` — optional shorter label used just in the sidebar list,
     for when the full title is long.
   - `date` / `link` — shown as a small clickable line under the heading.
5. Optional: name one photo `preview.jpg` (or `.png`/`.webp`) to control
   the cover image. Without one, the first photo found is used.
6. Optional: name one photo `banner.jpg` (or `.png`/`.webp`) for a
   low-opacity background image behind the section's title.
7. Optional: to control photo order (e.g. official capsule shots first,
   then found/stock photos, then fan pics), make three subfolders inside
   the collection folder named exactly `capsule`, `stock`, and `fan`, and
   sort your photos into them — they'll display in that order. If you
   skip this, every photo in the folder (and any subfolders) just shows in
   one flat grid, which is what all of your existing folders already do.
8. Commit and push. The site rebuilds and redeploys on its own.

---

## Getting your photos out of Google Sites

Google Sites won't let you bulk-export images, so this part is manual,
page by page, for each of your old sub-sites:

1. Open the sub-site in your browser.
2. Right-click each photo → **Save image as...** → save it into a
   collection folder as described above.
3. A browser extension like "Image Downloader" or "Fatkun Batch Download
   Image" can grab every image on a page in one click — much faster than
   doing it one at a time.

Also grab these small images from your original homepage the same way and
put them in `assets/img/`:
- your circular logo → save as `logo.jpg`
- the Instagram icon → save as `icon-instagram.png`
- the X icon → save as `icon-x.png`
- the "link" icon → save as `icon-link.png`

---

## Putting it on GitHub Pages

1. Create a free GitHub account if needed: https://github.com/join
2. Create a new repository (Public).
3. Upload everything in this project into that repo (drag the contents in
   via **Add file → Upload files** — make sure category folders like
   `drop/` land at the repo's top level, not nested inside anything else).
4. Go to **Settings → Pages**. Under "Build and deployment", set **Source**
   to **GitHub Actions** (not "Deploy from a branch").
5. Check the **Actions** tab — "Build and deploy site" should run and go
   green. Your site is then live at
   `https://yourname.github.io/your-repo-name/`.
6. Still in **Settings → Pages**, under "Custom domain" enter
   `www.g59mercharchive.xyz` and save (the included `CNAME` file also
   states this).

Note: `.github/workflows/deploy.yml` runs on pushes to a branch called
`main`. If your repo's default branch is `master` instead, open that file
and change `branches: [main]` to `branches: [master]`.

## Pointing your domain at GitHub

Wherever you bought g59mercharchive.xyz, open its DNS settings and add:

**Root domain** — four A records, all pointing to:
```
185.199.108.153
185.199.109.153
185.199.110.153
185.199.111.153
```

**www subdomain** — one CNAME record:
```
www  →  yourname.github.io
```

DNS changes can take minutes to a few hours. Once live, tick "Enforce
HTTPS" in **Settings → Pages**.

---

## Notes

- Nothing here needs installing or building on your own computer — GitHub
  does the building via `.github/workflows/deploy.yml`, which runs
  `scripts/build.mjs`. You never need to run it yourself.
- To preview locally before pushing (needs Node.js installed): run
  `node scripts/build.mjs` from this folder, then open `_site/index.html`
  in a browser.
- To rename a category, change its display order, or add a new one, edit
  `data/categories.json`. The `key` in that file must match the folder
  name at the repo root exactly.
