# Site Section Map

Built from ~903 reference frames (0.1s interval) extracted from the walkthrough screen
recording — frames live locally in `~/Documents/Reference images/oryzo-frames/`
(not committed, ~341 MB).

| # | Section | What happens |
|---|---------|--------------|
| 1 | Hero — desk scene | Top-down cutting-mat desk with the cork coaster, pencil, cutter, paperclips. Individual desk objects are separate `_color`/`_alpha` texture pairs composited in WebGL. Vimeo-embedded promo video in the corner. |
| 2 | "Powered by AI" | 3D hand holds the coaster (interactive — "try to hover hand"), gradient rim glow around the viewport. |
| 3 | Wearable | "So portable, it's wearable" — SplitText headline slides while a dashed-border video panel plays; wearable gallery (`/images/wearable-gallery/`) with clip videos + thumbs. |
| 4 | Coaster tumble | Dark transition; 3D coaster tumbles ("isn't just a coaster"); faded ORYZO wordmark transition. |
| 5 | Features | Live desk render (pinboard, keyboard, coffee cup on coaster) with copy panels; camera path driven by `featuresAnimations/*.buf`. |
| 6 | Cork macro | Full-bleed microscopic cork texture scroll (waterbear textures under `textures/table/`). |
| 7 | Product columns | Horizontal columns: drop-test ("-Tested", DATE/DAMAGE captions), sticker board, "Legacy" ancient vessels (Greece 500 BCE → Roman → Egypt → China 1100 BCE) each on a cork coaster. |
| 8 | Choose your own | "CHOOSE YOUR OWN" product picker. |
| 9 | Contact / footer | Contact + legal (privacy policy, terms PDFs). |

## Tech observed

- Astro static build; one hoisted JS bundle (Three.js + GSAP + ScrollTrigger + SplitText + Lenis-style smooth scroll, custom WebGL engine by Lusion).
- Gaussian splats (`/splats/*.sog` + `SplatsWorker` + `splat_sorter` wasm) for the table reflection/props.
- Rive (`/rive/oryzo.riv`) for vector animation; wasm runtime from CDN.
- Custom mesh format `.buf` under `/models/`; MSDF text (`fonts/msdf/Inter.*`).
- Desktop textures webp/avif; mobile falls back to `*_MOBILE.png/webp` variants.
- Fonts: local `DM-Mono`, `Literata` + Adobe Typekit (`use.typekit.net/pmn6ngx.css`, loaded externally).
- Video: Vimeo player embed (`player.vimeo.com/video/1174820580`), external.
