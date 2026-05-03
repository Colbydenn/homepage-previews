# DEA-1.3 — Preview Homepages

Two preview homepages built from DEA-1.2 enriched lead profiles. Static, dependency-free, deploy-ready for Cloudflare Pages.

## Sites

| # | Lead | Vibe | Hero | Confidence |
|---|------|------|------|------------|
| 1 | Three Fates Tattoo | mystical / feminine / art-historical | Canvas particle "Fates thread" field with cursor attraction | 0.80 |
| 2 | Tattoo Dumond | intimate / artisan / contemporary-dark | CPU Bayer-dither hero with cursor parallax warp | 0.78 |

## Tech

- Vanilla HTML + CSS, single-file per site
- GSAP 3.12.5 + ScrollTrigger (CDN)
- Lenis 1.0.42 smooth scroll (CDN)
- Self-contained signature interactive hero per site (Canvas2D — no WebGL build step needed)
- Google Fonts: Cormorant Garamond + Inter (Three Fates), Fraunces + Inter (Tattoo Dumond)

## Structure

```
previews/
├── index.html              ← gallery / index
├── _headers                ← CF Pages cache + security headers
├── three-fates/
│   ├── index.html
│   └── bg/                 ← 73 WebP frames (Kling 3.0 scroll-scrub)
└── tattoo-dumond/
    ├── index.html
    └── bg/                 ← 73 WebP frames (Kling 3.0 scroll-scrub)
```

## Background: Kling 3.0 scroll-scrub

Both pages use a single full-viewport `<canvas id="bgCanvas">` that draws one of 73 preloaded WebP frames per scroll position. Top of scroll = `f_001.webp` (start image), bottom = `f_073.webp` (end image). 1280px wide, ~1.4–2.0MB total per site, long-cached via `_headers`. Reduced-motion users see the first frame as a static poster.

Source frames generated via Higgsfield MCP:
- Nano Banana Pro 2k for start + end keyframes (16:9)
- Kling 3.0 std mode, 5s, start_image + end_image
- ffmpeg `fps=14.4,scale=1280` → libwebp q=70

Three Fates job IDs — start `e5ee074a` / end `364891c2` / video `ebcd17d2`.
Tattoo Dumond job IDs — start `5083d218` / end `a1397e10` / video `24c73e3b`.

## Library v0 status (DEA-6 dependency)

DEA-6 (Component Library Architect) is still `todo` at time of build. These previews are **self-contained** by design — when the library v0 ships, common organisms (nav, services, portfolio, process, testimonial, visit, CTA, footer) can be extracted from these two files and replaced with shared components in a follow-up pass. The signature interactive heroes stay per-site (niche-matched).

## Organisms used per site (5–8 each)

Three Fates: nav, hero (interactive), marquee, about, services, portfolio, process, testimonial, visit, CTA banner, footer = 11 (8 static)
Tattoo Dumond: nav, hero (interactive), intro+awards, services, portfolio, process, quote, visit, CTA, footer = 10 (8 static)

## Deploy

Cloudflare Pages — drag-drop or `wrangler pages deploy previews/`. Free `*.pages.dev` subdomain. No build step.

## Lighthouse target

Mobile ≥ 85, Desktop ≥ 90. Static HTML + minimal CDN deps; canvas heroes are GPU-light. To verify post-deploy:
```
npx lighthouse https://<subdomain>.pages.dev/three-fates/ --form-factor=mobile
npx lighthouse https://<subdomain>.pages.dev/tattoo-dumond/ --form-factor=desktop
```

## Source profiles

`../lead-enrichment-top10.json` (top 2 selected: indices 2 and 3 — Three Fates + Tattoo Dumond, the highest-confidence weak-website candidates).
