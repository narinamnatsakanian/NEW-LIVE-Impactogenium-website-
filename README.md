# Impactogenium Website

Single-page marketing site for Impactogenium, an independent impact investing advisory practice led by Narina Mnatsakanian.

## What this is

The entire site — every page, all styling, and all images — lives in **`index.html`**. It's a self-contained single-page app:

- Client-side hash routing (`#home`, `#services`, `#services-investors`, `#services-startups`, `#services-ecosystem`, `#clients`, `#about`, `#contact`, `#privacy`) renders different page templates into `#app` — there's no build step and no external dependencies.
- All photography is embedded directly as base64 data URIs inside the file, so the page works from a single static file with no separate `/images` folder to keep track of.
- Fonts load from Google Fonts (Playfair Display); everything else is inline CSS/JS in the one file.

## Running it locally

No build tools, no `npm install`. Just open the file, or serve it:

```bash
# Option 1: just open it
open index.html

# Option 2: serve it (recommended, avoids any local file:// quirks)
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Deploying

Because it's a single static HTML file, it can be deployed anywhere that serves static files:

- **GitHub Pages** — push this repo to GitHub, then enable Pages (Settings → Pages → Deploy from branch → `main` / root). The site will be live at `https://<username>.github.io/<repo>/`.
- **Netlify / Vercel** — drag-and-drop the folder, or connect the GitHub repo for automatic deploys on push. No build command needed (or set it to a no-op).
- **Any static host** (S3 + CloudFront, Cloudflare Pages, etc.) — just upload `index.html`.

For a custom domain (e.g. `www.impactogenium.com`), point the domain's DNS at whichever host you choose and configure the custom domain in that host's settings.

## Editing

Because everything is in one file, the easiest way to make further changes is with [Claude Code](https://claude.com/claude-code) (or another AI coding assistant) pointed at this repo — open this folder in Claude Code and describe the change you want (copy edits, new sections, photo swaps, layout tweaks). For hand-editing, `index.html` is organized top to bottom as:

1. `<head>` — meta tags, SEO, structured data
2. `<style>` — all CSS, using a small set of design tokens (colors, fonts) near the top
3. Photo constants (`const PHOTO_...`) — base64-embedded images
4. `TEMPLATES` — one JS template-literal function per page
5. Routing and render logic at the bottom

## Contact form

The contact form currently submits via a `mailto:` link. If you want form submissions to land somewhere more reliable (an inbox, a CRM, a spreadsheet), swap it for a service like Formspree, Basin, or a simple serverless function — that's a small, contained change.
