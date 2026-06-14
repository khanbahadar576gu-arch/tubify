# Tubify website

Static landing + download page for Tubify. Single `index.html`, no build step. Styled to match the
app (Poppins, brand red `#FF0033`, rounded cards) and uses the Tubify brand mark as the logo. The
site is always **light mode**, regardless of the device theme.

**Mobile-first:** the layout is built for phones (where downloads happen) — single-column cards,
large thumb-friendly tap targets, safe-area insets for notched/gesture-nav devices, and a
**persistent bottom download bar** that one-taps the recommended build and slides away when the
download section is on screen. It scales up to a multi-column desktop layout above 720px.

```
website/
  index.html       the page (self-contained; only external dep is the Poppins web font)
  privacy.html     full privacy policy (linked from the footer + sitemap)
  logo.png         Tubify brand mark (nav + footer) — transparent red "T" download arrow
  favicon.webp     round launcher icon (browser tab icon)
  robots.txt       allows all crawlers, points to the sitemap
  sitemap.xml      single-page sitemap for search engines
  site.webmanifest web app manifest (name, theme color, icon)
  .nojekyll        tells GitHub Pages to serve files as-is
```

> No `CNAME` file is included because the `tubify.pro` domain isn't registered yet — the site runs
> on a `*.github.io` address until you buy a domain (see "Publish" below).

## SEO
The page is search-optimized: keyword-tuned `<title>`/description, `canonical`, Open Graph + Twitter
cards, and two **JSON-LD** blocks (`SoftwareApplication` + `FAQPage`) for rich results, plus a
visible FAQ, `robots.txt`, and `sitemap.xml`.

- **Absolute URLs are placeholders set to `https://tubify.pro/`** (the planned domain — **not yet
  registered**) in `index.html` (canonical, og:url, og:image, twitter:image, JSON-LD
  `url`/`downloadUrl`), `robots.txt`, and `sitemap.xml`. **Before deploying, find-and-replace
  `https://tubify.pro/`** with your real URL — your GitHub Pages address
  (`https://OWNER.github.io/REPO/`) for now, or `tubify.pro` once you buy it.
- After deploying, submit the site + `sitemap.xml` in **Google Search Console** to get indexed.
- Optional: replace the OG/Twitter `og:image` (currently the square `logo.png`) with a 1200×630
  banner for nicer link previews.
- No fake review/rating data is included (Google penalizes that) — add `aggregateRating` to the
  `SoftwareApplication` JSON-LD only if you have real reviews.

## Before publishing — edit `index.html`
1. Replace **`OWNER/REPO`** in the three `app-arm64-v8a-release.apk` links (hero button, download
   section, bottom dock) with your GitHub repo
   (e.g. `https://github.com/your-username/tubify/releases/latest/download/...`).
2. Update the **version** (`v1.5.2`) and **size** (`66 MB`) when you ship a new build.
   The site ships **only the arm64-v8a build** (~66 MB) — modern phones (2018+). To support
   32-bit / other devices later, add those APKs and links back.

All three buttons carry the `download` attribute and point at GitHub Release assets, which are served
as `Content-Disposition: attachment` — so a tap downloads the `.apk` directly (no intermediate page).

## Host the APKs (GitHub Releases — free, handles big files)
Don't put the APKs in this repo. Instead:
1. Push this `website/` folder to a GitHub repo.
2. **Releases → Draft a new release**, tag e.g. `v1.5.2`, and **upload `app-arm64-v8a-release.apk`** as an asset.
3. `releases/latest/download/<file>` (used in `index.html`) always resolves to the newest release.

## Publish the page (GitHub Pages)
1. Repo **Settings → Pages** → Source: **Deploy from a branch** → `main` / root (or `/website` if
   the site isn't at the repo root).
2. **No custom domain yet (current state):** there's no `CNAME` file, so the site lives at
   `https://OWNER.github.io/REPO/`. Use that URL for the find-and-replace above, and set Remote
   Config `update_url` to it so the in-app "Update" screen lands here.
3. **Later, when you buy `tubify.pro`:** add a `CNAME` file containing `tubify.pro`, then at your
   registrar add the DNS records GitHub shows (4 `A` records to GitHub's IPs, or a `CNAME` to
   `OWNER.github.io`). Enable **Enforce HTTPS** once the cert provisions, and swap the URLs back to
   `https://tubify.pro/`.

> `update_url` in Firebase Remote Config (default `https://tubify.pro`) should point here so the
> in-app "Update required" screen lands on this page.
