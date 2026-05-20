# ALD & Associates — Netlify Deployment

A complete, production-ready static site for ALD & Associates LLC.

## What's in this folder

```
/
├── index.html                      # Homepage
├── policies-procedures.html        # Practice page
├── program-management.html         # Practice page
├── team-development.html           # Practice page
├── leader-development.html         # Practice page
├── strategic-planning.html         # Practice page
├── training-programs.html          # Practice page
├── 404.html                        # Branded 404 page
├── netlify.toml                    # Netlify config (headers, caching, pretty URLs)
├── _headers                        # Backup header rules
├── _redirects                      # URL routing and legacy redirects
├── robots.txt                      # Search engine crawler rules
├── sitemap.xml                     # Search engine page index
├── favicon.png                     # 32x32 favicon
├── favicon-64.png                  # 64x64 favicon
├── favicon-192.png                 # 192x192 favicon / Apple touch icon
├── favicon-512.png                 # 512x512 favicon
├── og-image.png                    # 1200x630 Open Graph social card
└── README.md                       # This file
```

## Before you deploy — three things to do

### 1. Add your training catalog PDF

The homepage links to `/training-catalog.pdf`. Drop that file into this folder before deploying. The filename matters; if it's different, edit the `<a href>` references in `index.html` to match.

If you don't want the catalog hosted on the site itself, replace those URLs with wherever it lives (Google Drive, Dropbox, etc.).

### 2. Confirm the production domain

The meta tags, sitemap, and canonical URLs all reference `https://www.ald-associates.com`. If you're deploying to a different domain (Netlify subdomain, www vs apex, etc.), do a global find-and-replace before publishing. Search across all `.html`, `sitemap.xml`, `robots.txt`, and `netlify.toml`.

### 3. Replace the placeholder Capability Statement URL

The CTA section and utility links point to:
`https://ald-associates.com/wp-content/uploads/2026/05/ALD-Capability-Statement.pdf`

That URL assumes the PDF still lives on the existing WordPress site. If you're migrating fully off WordPress, host the PDF on Netlify (drop it in this folder as `capability-statement.pdf`) and update all references.

## Deploying — pick one method

### Method A — Netlify drag-and-drop (fastest)

1. Go to https://app.netlify.com/drop
2. Drag this entire folder onto the page
3. Wait ~30 seconds
4. You'll get a random URL like `wandering-pony-12345.netlify.app`
5. Add your custom domain under **Site settings → Domain management**

### Method B — Connect to GitHub (recommended for ongoing updates)

1. Push this folder to a private GitHub repository
2. In Netlify: **Add new site → Import from Git**
3. Select your repo
4. Build settings: leave blank (it's static; `netlify.toml` handles config)
5. Publish directory: `.` (the root)
6. Click **Deploy**

Every git push automatically rebuilds the site.

### Method C — Netlify CLI

```bash
npm install -g netlify-cli
netlify deploy --dir=. --prod
```

## Post-deployment checklist

After it's live, do these in order:

1. **Open the homepage in a fresh incognito window.** Check that the hero gradient renders, the SBA/GSA badges show, and the catalog cover is clickable.
2. **Click every practice page link from the homepage.** Verify the "back to Practices" breadcrumb returns you to the home services section.
3. **Open the mobile view (or your phone).** Open the hamburger menu, verify all links work, confirm the slide-out drawer closes when tapping outside.
4. **Hit a fake URL** like `/does-not-exist`. You should land on the branded 404, not Netlify's default.
5. **Share the homepage URL in Slack or iMessage.** The Open Graph preview should show the branded card with "Where institutions turn when transformation cannot fail."
6. **Run Lighthouse** (in Chrome DevTools). Expect 95+ on Performance, Accessibility, Best Practices, and SEO.
7. **Submit the sitemap** to Google Search Console (`/sitemap.xml`).

## Maintenance — what to know

**Updating copy or images**: edit the HTML directly. Each page is a single self-contained file (logo, photos, and OG cards are base64-embedded). No build step.

**Adding a new practice page**: the practice pages were generated from a Python template (`build_practice_pages_v2.py`, kept in the working environment, not in deploy folder). To add a 7th practice, regenerate from that template; or hand-edit any existing practice file as a starting point and add it to the homepage services grid, the footer practice column, the sitemap, and the related-practices cross-links on the other practice pages.

**Changing the brand red**: it's a CSS variable. Open `index.html`, find `--red:#8C1414` and replace. Repeat across the practice pages and `404.html`. Or pipe everything through a sed command if comfortable.

**Replacing the logo**: the logo is base64-embedded as `data:image/png;base64,...` inside the nav and footer of every page. If you need to swap the logo file, you'll want to re-encode and find-replace. Easier path: replace the `<img>` tag with `<img src="/logo.png">` and host the actual PNG in this folder.

## Known notes

- `og-image.png` was generated programmatically. It's clean and on-brand, but if you want a designer-built social card, replace the file (keep the 1200×630 dimensions and the filename).
- The favicon uses just the figure mark from the logo (the person silhouette). If you want the full wordmark in the favicon, you'll have to accept it being unreadable at 32×32 — almost no one does this.
- The Capability Statement URL still points to the WordPress site. If/when you fully migrate, host the PDF here and update the links.
- The Featured Engagement quote on each page is illustrative ("reference available under NDA"). Swap with real testimonials when you have written permission.
- One leader card is missing for Sharon (per the previous instruction). If you want her back on the page with a photo, drop her headshot in and I'll add the card back.

## Contact for handoff questions

For anything that doesn't make sense in this codebase, the original generation context is preserved — describe what you're trying to do and the existing structure can be extended.

---

© 2026 ALD & Associates LLC
