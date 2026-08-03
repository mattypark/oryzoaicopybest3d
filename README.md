# oryzoaicopybest3d

Educational 1:1 study clone of [oryzo.ai](https://oryzo.ai) — a mirror of the site's deployed static build, made to study how its WebGL scenes, Gaussian splats, GSAP/ScrollTrigger/SplitText animations, and scroll choreography work.

**All design, copy, images, video, 3D assets, and code belong to Oryzo / [Lusion](https://lusion.co) (the studio that built it).** This repo exists purely for learning purposes and is not affiliated with, endorsed by, or intended to impersonate Oryzo.

## Run

```bash
npx serve .
# or
python3 -m http.server 4173
```

Then open the printed localhost URL. Needs internet for the few external pieces below.

## What's mirrored

- `index.html` + `_astro/` — exact compiled Astro build (CSS, main JS bundle, splat worker, splat-sorter wasm). Only change: Cloudflare analytics beacon removed.
- `textures/`, `models/`, `splats/`, `rive/`, `images/`, `fonts/`, `meta/` — every asset the site loads on desktop and mobile, captured via full-page network recording (zero 404s on both viewports).

## Still external (same as the live site)

- Vimeo promo video (`player.vimeo.com/video/1174820580`)
- Adobe Typekit fonts (`use.typekit.net/pmn6ngx.css`)
- Rive wasm runtime (unpkg CDN)

No placeholders were needed — every self-hosted asset downloaded successfully.

## Docs

- [`docs/SECTIONS.md`](docs/SECTIONS.md) — section-by-section map of the page + tech breakdown.

## Build stages

- [x] Stage 1 — Reference frames from screen recording (local only)
- [x] Stage 2 — Repo scaffold
- [x] Stage 3 — Mirror core build files
- [x] Stage 4 — Harvest all statically-referenced assets
- [x] Stage 5 — Runtime network capture, mobile variants, zero-404 verification
- [x] Stage 6 — Docs + polish
