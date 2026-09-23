# OEZ Works (oezworks.com)

Static hub site for OEZ Works: free browser-only web tools for online sellers,
plus usage guides and a blog.

## Structure

- `/` — hub homepage (site intro, service cards, latest posts)
- `/about/`, `/privacy/`, `/contact/` — shared site pages (single source at root)
- `/blog/` — devlogs, update notes, how-to articles
- `/converter/` — Detail HTML Converter tool
- `/converter/guide/` — converter usage guides
- `/calculator/` — 1688 import cost calculator (static build from the private `icm` repo — do not edit by hand)
- `/calculator/guide/` — cost calculator usage guides (maintained here)
- `/converter/about|privacy|contact/` — redirect stubs to the root pages (kept for old indexed URLs)

## Updating the cost calculator

The calculator bundle is built in the `icm` repo:

```bash
VITE_APP_URL=https://<원가노트 app URL> pnpm --filter web build:calculator
```

Then replace `calculator/index.html` and `calculator/assets/` with the contents of
`apps/web/dist-calculator/`. Leave `calculator/guide/` untouched.

## Deploy

1. Deploy this folder to GitHub Pages (current), Vercel, Netlify, or Cloudflare Pages.
2. Connect a custom domain. Current production domain: `oezworks.com`.
3. Submit `/sitemap.xml` to Google Search Console after deployment.

## AdSense

- Publisher: `ca-pub-2667365474859426` (`ads.txt` at root).
- The AdSense loader snippet is installed in the `<head>` of every content page
  (root pages, blog, converter, guides). Redirect stubs carry no ad code.

## Notes

- The converter runs entirely in the browser; user input is not sent to a backend.
- External image URLs in converted HTML may still be blocked by the target sales channel.
- Social channels linked on `/about/`: Instagram/TikTok `@oez.maker`, YouTube `@오이지빌더`.
