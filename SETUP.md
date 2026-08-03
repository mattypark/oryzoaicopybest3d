# SETUP.md — Run This On Localhost

Zero build steps. It's a static site — you just need to serve the folder.

## First time (or on a new machine)

```bash
# 1. Get the repo
git clone https://github.com/mattypark/oryzoaicopybest3d.git
cd oryzoaicopybest3d

# 2. Serve it (pick ONE — no installs needed for the python option)
python3 -m http.server 4173        # built into macOS
# or
npx serve .                        # if you prefer node (auto-downloads serve)

# 3. Open it
open http://localhost:4173
```

That's it. No `npm install`, no build, no env vars, no database.

## Running it again later

```bash
cd ~/Downloads/current-projects/oryzoaicopybest3d
python3 -m http.server 4173
```

Then open `http://localhost:4173` in any browser. Stop the server with `Ctrl+C`.

## Why you MUST use a server (not double-click index.html)

Opening `index.html` directly uses `file://` — the browser blocks the wasm,
web-worker, and texture fetches the engine needs (CORS). Always serve over
`http://localhost`.

## Requirements

- Any modern browser with WebGL2 (Chrome/Safari/Arc/Firefox — all fine).
- **Internet connection** for three external pieces the real site also loads
  externally: the Vimeo promo video, Adobe Typekit fonts, and the Rive wasm
  runtime from CDN. Everything else is local — offline the site still runs,
  those three just fall back/skip.

## Troubleshooting

| Symptom | Fix |
|---|---|
| Port busy (`Address already in use`) | Use another port: `python3 -m http.server 4174` |
| Black screen / nothing renders | Hard refresh (`Cmd+Shift+R`); check DevTools console — should be zero red 404s |
| Fonts look slightly off | Typekit blocked/offline — needs internet |
| Promo video missing | Vimeo embed — needs internet |
| Scroll feels stuck at "SCROLL TO CONTINUE" | Normal — keep scrolling, sections snap |

## What's where (repo map)

```
index.html            entry — the whole page
_astro/               compiled CSS + the 1.1MB engine bundle + splat worker/wasm
textures/ models/     baked renders, meshes, animation buffers
splats/               Gaussian splat scenes (.sog)
images/ fonts/ rive/  section media, local fonts, Rive animation
assets/screenshots/   README screenshots (not used by the site)
docs/SECTIONS.md      section-by-section map of the page
REBUILD.md            guide for recreating this from scratch with your own assets
```
