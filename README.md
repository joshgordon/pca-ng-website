# PCA NextGen Conference

A Hugo link-in-bio site with links and info for PCA's NextGen conference, served at
https://pca-ng.org by Cloudflare Pages. Pushing `main` deploys production; every other branch
gets a preview URL.

Cloudflare Pages build settings: build command `hugo`, output directory `_site` (set by
`publishDir` in `hugo.toml`), and the environment variable `HUGO_VERSION` set to a recent Hugo
(0.146 or newer; built with 0.167.0).

## Local development

```bash
hugo server
```

Then open `http://localhost:1313`.

## Configuration

Edit `hugo.toml` to update the site title, description, favicon, and other settings.

The header logos are inline SVGs (so the `#blue` fill can change in dark mode):

- `layouts/_partials/logo.html` — home page logo
- `layouts/_partials/logo-small.html` — logo on sub-pages

## Adding / editing links

Edit `data/links.yml`. Each category has a list of items:

```yaml
- category: Category Name
  items:
    - title: Link Title
      url: https://example.com
      icon: mdi:some-icon   # optional, from https://icon-sets.iconify.design/
      tag: New              # optional badge label
```

Items without a `url` render as a non-clickable button (useful for "coming soon" placeholders).
External links open in a new tab; internal links open in the same tab.

PDFs and other static files go in `static/assets/` and are referenced as `/assets/filename.pdf`.

## Pages

Any Markdown file in `content/` becomes a page (e.g. `content/bookstore.md` → `/bookstore/`).
Link to it from `data/links.yml` with `url: /bookstore/`. A folder of pages in `content/`
gets an index page listing them.

The conference handouts are pages styled to print like the original PDFs (logo, letter size,
brand colors; the map prints landscape). Use the browser's Print to get a handout.

- `content/schedule.md` — rendered from `data/schedule.yml`. A `seminars: "10:30"` event pulls
  that slot's sessions (titles, rooms, links) from `data/seminars.yml`.
- `content/seminars.md` — rendered from `data/seminars.yml`. Optional per-seminar fields for the
  schedule: `short` (shorter title), `who` (shown in parentheses), `short_location`.
- `content/st-louis.md` — plain Markdown (things to do, coffee, restaurants).
- `content/map.md` — floor plan of The Commons drawn as SVG in `layouts/_shortcodes/map.html`,
  plus the room legend.

Page front matter options (under `params:`):

- `class` — added to `<body>` for page-specific styles
- `subtitle` — line under the title
- `print_logo` — logo printed above the title: `conference` (default), `cdm`, or `false` for none
- `print_title: false` — hide the title when printed

Shortcodes available in pages:

- `{{< addr "123 Main St, Town, MD" >}}` — address that links to Google Maps (plain text when printed)
- `{{< place "Name" >}}` — place name that links to a Google Maps search (`"Name, St. Louis, MO"`
  unless a second argument gives the search)
- `{{< legend >}}…{{< /legend >}}` — Markdown laid out in columns (map room legend)
- `{{< print-logo >}}` / `{{< print-logo "cdm" >}}` — logo that only appears when printed
- `{{< checklist >}}` — renders a list with checkboxes (packing lists)
- `{{< dontbring >}}` — renders a list with X bullets
- `{{< day "monday" >}}…{{< /day >}}` — collapsible section per day; add `{{< open-today >}}` to auto-expand today and later days

## Optional features

These partials exist but are commented out in `layouts/home.html`. Uncomment to enable:

- `theme-switch.html` — dark/light mode toggle button
- `social.html` — social media icon bar (reads from `data/social.yml`)
- `bio.html` — Instagram/TikTok/YouTube link-in-bio grid (reads from `data/bio.yml`)
- `footer.html` — copyright footer
