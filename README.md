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

The calculator bundle is built in the private `zihopark/icm` repo:

```bash
VITE_APP_URL=https://<원가노트 app URL> pnpm --filter web build:calculator
```

Then replace `calculator/index.html` and `calculator/assets/` with the contents of
`apps/web/dist-calculator/`. Leave `calculator/guide/` untouched.

`VITE_APP_URL` is the deployed 원가노트 member app. It is not deployed yet, so the current
bundle was built with `VITE_APP_URL=https://oezworks.com` and then hand-patched in
`calculator/assets/calculator-*.js` to hide the links that would 404 there
(서비스 소개 / 로그인 / 무료 회원가입 in the header, and the PERSONAL WORKSPACE banner):

- `function eu(...)` (external app link) returns `null` when a site config is set
- the `PERSONAL WORKSPACE` `<section>` is prefixed with `false&&`

A rebuild drops these patches. Re-apply them, or better, make `icm` hide those links
when no app URL is configured, until the member app is live.

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
