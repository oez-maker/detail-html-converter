# OEZ Works (oezworks.com)

Static hub site for OEZ Works: free browser-only web tools for online sellers,
plus usage guides and a blog.

## Structure

- `/` — hub homepage (tools, guides, latest posts)
- `/converter/`, `/calculator/` — the tools
- `/guide/` — guide hub listing every tool's guides
- `/converter/guide/`, `/calculator/guide/` — per-tool guide index; each tool has
  a `how-to-use/` start guide and a `faq/` page
- `/blog/` — devlogs, update notes, how-to articles
- `/about/`, `/privacy/`, `/contact/` — shared site pages
- `/converter/about|privacy|contact/` — redirect stubs to the root pages (kept for old indexed URLs)

`/calculator/index.html` and `/calculator/assets/` are a static build from the private
`zihopark/icm` repo — do not edit by hand (see below).

### Page conventions

Every static page uses the same header (logo → `/`, nav: HTML 변환기 · 원가 계산기 · 가이드 ·
블로그 · 소개) and the same footer. The current nav item gets `aria-current="page"`; the
section a page belongs to gets `aria-current="true"` (all tool guides belong to 가이드).
Guide pages start with a breadcrumb (가이드 › tool › page) and end with a CTA to the tool.
When adding a guide, also add it to the tool's guide index, the `/guide/` hub, the
article sidebar of that tool's guides, and `sitemap.xml`.

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
