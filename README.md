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

## Deploying to Netlify (recommended — this repo is set up for it)

`netlify.toml` is already included (`publish = "."`, no build command needed), so either of these works:

1. **Drag-and-drop**: go to [app.netlify.com/drop](https://app.netlify.com/drop) and drag this folder in. Netlify serves `index.html` immediately at a generated `*.netlify.app` URL.
2. **Connect the GitHub repo** (recommended for future edits): push this repo to GitHub, then in Netlify choose "Add new site → Import an existing project" and pick the repo. Every push to `main` redeploys automatically.

Either way, **one manual step in the Netlify dashboard is required after the first deploy** to make the contact form actually email you (see below) — nothing in this repo can set that up, since it's an account-level setting, not a file.

For a custom domain (e.g. `www.impactogenium.com`), add it under Site settings → Domain management and point your domain's DNS as Netlify instructs.

Other static hosts (GitHub Pages, Vercel, S3 + CloudFront, Cloudflare Pages) also work since this is one static file — but only Netlify will run the form-handling described below without extra setup.

## Editing

Because everything is in one file, the easiest way to make further changes is with [Claude Code](https://claude.com/claude-code) (or another AI coding assistant) pointed at this repo — open this folder in Claude Code and describe the change you want (copy edits, new sections, photo swaps, layout tweaks). For hand-editing, `index.html` is organized top to bottom as:

1. `<head>` — meta tags, SEO, structured data
2. `<style>` — all CSS, using a small set of design tokens (colors, fonts) near the top
3. Photo constants (`const PHOTO_...`) — base64-embedded images
4. `TEMPLATES` — one JS template-literal function per page
5. Routing and render logic at the bottom

## Contact form — wired to Netlify Forms

The Contact page form submits via [Netlify Forms](https://docs.netlify.com/manage/forms/setup/) — no external service, no serverless function, and no code to maintain. Submissions land in the Netlify dashboard and (once you complete the one-time step below) get emailed to you automatically.

Two things make this work, both already in place in `index.html`:

1. A **hidden static copy of the form** sits right after `<main id="app">`, with `name="contact"` and `data-netlify="true"`. This is required because the real, visible form only exists in the DOM after JavaScript renders the Contact page — Netlify's form-detection crawler reads only the literal HTML in the deployed file at build time, so without this static twin it would never register a "contact" form at all.
2. The real form (on the Contact page) intercepts its own submit with `fetch()` and posts the same field names to `/` — so visitors never leave the page or see a `mailto:` prompt, and there's an inline "Sending… / Thank you / error" status message.

A hidden honeypot field (`bot-field`) is included on both forms for basic spam filtering — this is a no-config Netlify feature (`netlify-honeypot="bot-field"`), not something you need to manage.

**One-time setup you need to do, after the first deploy** (this is an account setting Netlify keeps outside the code, so it can't ship pre-configured in this repo):

1. In the Netlify dashboard, open your site → **Forms**.
2. Confirm a form named **"contact"** appears in the list (it will, once the site has deployed at least once with this `index.html`).
3. Go to **Site settings → Forms → Form notifications → Add notification → Email notification**.
4. Set it to notify **narina.mnatsakanian@gmail.com** on new submissions, and save.

After that, every inquiry submitted on the live site lands in your Gmail inbox automatically. (Netlify's free tier includes 100 form submissions/month, which resets monthly — worth knowing if inquiry volume ever grows.)

## Booking calendar

The "Open Booking Calendar" button on the Contact page already links directly to your Google Calendar appointment page (`calendar.app.google/Qsj8Whsvd2qwV9zLA`) — no setup needed here. If bookings ever need to go to a different calendar or link, that's a one-line change in `index.html` (search for `calendar.app.google`).
