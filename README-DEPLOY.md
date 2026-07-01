# Errored Coin — Cloudflare Pages Deployment Package

## What's in this folder
- index.html, about.html, services.html, error-types.html, who-we-serve.html,
  expert-panel.html, capital.html, contact.html — your 8 pages, design and text
  unchanged, with SEO/AI-crawler meta tags and structured data added to each <head>
- robots.txt — explicitly allows Google, Bing, GPTBot, ClaudeBot, PerplexityBot,
  Google-Extended, CCBot, and other major AI crawlers
- sitemap.xml — lists all 8 pages for search engine discovery
- llms.txt — a plain-text summary of the site for AI browsers/agents (ChatGPT,
  Copilot, Gemini, Perplexity increasingly read this file directly)
- _redirects — clean-URL rules (e.g. /home → /, /about.html → /about)

## How links are built (important for this GitHub + Cloudflare setup)
- **Internal navigation** (header nav, footer nav, in-page CTAs like "Start a
  Conversation") uses **root-relative paths**: `/services`, `/about`,
  `/contact`, etc. — not the full domain. This matters because Cloudflare
  Pages also gives you a preview URL for every branch/PR (e.g.
  `abc123.errored-coin.pages.dev`). Root-relative links keep working
  correctly on those preview deploys too; hardcoded `https://www.errored.co.in/...`
  links would incorrectly send preview visitors to the live production site.
- **Canonical tags, Open Graph tags, and JSON-LD structured data** (the
  metadata search engines and AI crawlers read) still use the full
  `https://www.errored.co.in/...` URL for each page, since that's required
  by spec — these are the only places the absolute domain appears in the code.
- **Social links** (LinkedIn, YouTube, Instagram, X, Facebook) are full
  external URLs, as they must be.

## Step 1 — Push to GitHub
1. Create a repository (e.g. `errored-coin-website`) on GitHub.
2. Add all files from this package to the repo **root** — not in a subfolder:
   the 8 `.html` files, `robots.txt`, `sitemap.xml`, `llms.txt`, `_redirects`.
3. Commit and push to `main`.

## Step 2 — Connect Cloudflare Pages to the repo
1. https://dash.cloudflare.com → Workers & Pages → Create → Pages →
   **Connect to Git**.
2. Authorize GitHub, select the `errored-coin-website` repo.
3. Build settings: Framework preset **None**, Build command *blank*,
   Build output directory `/`.
4. Deploy. Cloudflare gives a `*.pages.dev` URL immediately, and every
   future `git push` to `main` auto-redeploys — no manual re-upload needed.

## Step 3 — Connect the custom domain
1. Project → **Custom domains** → **Add a custom domain** → enter
   `errored.co.in`, then repeat for `www.errored.co.in`.
2. If your domain's DNS is already on Cloudflare, this activates
   automatically. If it's registered elsewhere, Cloudflare shows the exact
   DNS records to add at your registrar.
3. Once DNS propagates, the live site is served directly — no iframe, no
   Google Sites limitation.

## How to update the site later (once connected to GitHub)
You do **not** need to re-upload a zip again. Any of these work:
- **On GitHub.com directly**: open the file (e.g. `services.html`) in your
  repo → click the pencil (Edit) icon → make changes → **Commit changes**.
  Cloudflare auto-redeploys within a minute or two.
- **Replace a file**: repo → Add file → Upload files → drag the new version
  of the file in (same filename) → commit. GitHub overwrites it.
- **From your computer with git installed**: edit the files locally, then
  ```
  git add .
  git commit -m "update services page"
  git push
  ```
Either way, Cloudflare watches the `main` branch and rebuilds automatically
on every push — you'll see the deploy appear in the Cloudflare Pages
dashboard within seconds of pushing.

## After going live — do these once
1. Add an actual Open Graph image at /og-image.png (1200x630px) — currently
   referenced in meta tags but not included in this package. Until you add
   it, remove or ignore those two og:image / twitter:image lines, or upload
   a branded image at that exact path.
2. Google Search Console → add property for www.errored.co.in → submit
   https://www.errored.co.in/sitemap.xml
3. Bing Webmaster Tools → same, submit the sitemap (this also feeds Copilot).
4. Because pages are now real static HTML (not iframe-embedded), you no
   longer need the Google Sites framework-hub workaround for crawlability —
   every page here is directly indexable and citable on its own. You can
   still keep those framework pages if you like them for other reasons, but
   they're no longer structurally necessary for SEO/AI visibility.

## Ongoing edits
Any time you change a page's text, edit the .html file directly and
re-upload (drag-and-drop again, or connect a GitHub repo in Cloudflare Pages
for automatic redeploys on every git push — recommended once the site is
stable, since it also gives you version history).
