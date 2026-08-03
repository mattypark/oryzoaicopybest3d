# REBUILD.md — Build Your Own Version From Scratch

> Handoff doc for a fresh Claude Code session. Goal: recreate this *kind* of site —
> the scroll-driven, baked-3D, film-quality product page — as an **original project**
> with your own product, your own assets, your own code. Nothing from Oryzo/Lusion
> gets reused. This mirror repo is your *reference for structure and technique only*.

---

## 1. What this repo is (context for the new session)

- `mattypark/oryzoaicopybest3d` = byte-perfect mirror of https://oryzo.ai, built by
  downloading their compiled build + recording every network request headlessly.
- Their engine is ONE minified 1.1 MB bundle (custom WebGL engine by Lusion +
  GSAP/ScrollTrigger/SplitText + custom smooth scroll). It is NOT editable source —
  that's why the rebuild starts from scratch.
- Reference material available to the new session:
  - This repo's file tree — shows *what kinds* of assets such a site needs
    (`textures/`, `models/`, `splats/`, section images, MSDF font data).
  - `docs/SECTIONS.md` — section-by-section map of the page + tech notes.
  - `README.md` — how each animation trick works.
  - `~/Documents/Reference images/oryzo-frames/` — 903 frames (0.1s apart) of the
    full scroll-through. Use for pacing/choreography reference, NOT for copying art.
- **Rule for the rebuild: study the choreography, replace every asset and all copy
  with your own.** New product, new name, new textures, new renders, new text.

## 2. What I could copy vs. couldn't — and what that means for you

| Piece | Mirror status | From-scratch replacement |
|---|---|---|
| HTML/CSS/JS engine | Copied but minified/unreadable | Write your own (stack below) |
| Baked textures & renders | Copied | Render your own in Blender |
| `.buf` 3D meshes + baked animations | Copied but proprietary format | Export your own glTF/DRACO from Blender |
| Gaussian splats (`.sog`) | Copied but proprietary format | Capture/train your own splats, or fake with planar reflections |
| Adobe Typekit fonts | **Could NOT copy** (licensed, loads from Adobe) | Use open fonts (Google Fonts / Fontshare) |
| Vimeo promo video | **Could NOT copy** (external embed) | Shoot/render your own, self-host mp4 |
| Rive `.riv` animation | Copied but needs Rive editor to author | Make your own in Rive (free tier) or use Lottie |
| Site copy/text | Copied | Write your own jokes/copy |

## 3. The stack (matches your standard animation stack)

```
Vite + vanilla TS (or Astro)        → static output, fast dev
three (Three.js)                    → WebGL scenes
gsap + ScrollTrigger                → scroll-driven timelines
SplitText: gsap SplitText           → per-char text reveals
lenis                               → smooth virtual scroll
(optional) @rive-app/canvas         → vector sprite animations / easter eggs
(optional) troika-three-text        → MSDF text inside WebGL (their trick)
```

Wire Lenis → ScrollTrigger with `lenis.on('scroll', ScrollTrigger.update)` and
`scrollerProxy`. Every 3D camera + timeline is driven by one scroll progress value.

## 4. The one core principle

**Bake, don't simulate.** Their film look = offline renders played back in WebGL,
not heavy real-time lighting. Blender (free) is the asset factory. Real-time layer
stays thin: composite layers, scrub baked animations, add parallax + postFX.

## 5. Per-section recipes (how to MAKE each thing yourself)

### 5.1 Desk hero (their welcome screen — the one you love)
1. Blender: model/kit-bash a desk scene (cutting mat, your product, 2–3 props).
   Free assets: polyhaven.com (CC0 HDRIs, textures, models) — license-safe.
2. Render ONE high-res background plate (Cycles, 2560px+).
3. Render each foreground prop SEPARATELY with transparent film → export as
   webp pairs: color + alpha (or single RGBA webp).
4. Web: fullscreen quad shows the plate; each prop is a textured plane at a
   slightly different Z. Mouse move → tiny offset per layer (parallax). Done —
   looks photoreal because it IS a photoreal render.

### 5.2 Scroll-scrubbed 3D showpiece (their coaster tumble / features camera fly)
1. Blender: animate your product tumbling + a camera path (keyframes).
2. Export glTF **with animations** (their `.buf` ≈ this, custom-packed).
3. Three.js: load glTF, create `AnimationMixer`, then
   `mixer.setTime(scrollProgress * clip.duration)` inside a ScrollTrigger
   `onUpdate`. Scroll = timeline scrub. That's the entire trick.
4. Lighting: bake it — either bake lightmaps in Blender, or use `MeshBasicMaterial`
   with fully-lit baked textures so WebGL does zero lighting work.

### 5.3 Photoreal room scene (their coffee/pegboard desk)
- Same as 5.2 but the scene is mostly static meshes with baked-lighting textures;
  only the camera moves (scrubbed path) + one or two hero objects animate.
- Smoke: render a short smoke sim (Blender Mantaflow) to a sprite sheet, play it
  on a billboard plane. Skip their spherical-harmonics relighting v1 — flipbook
  already reads great.
- Reflections: their splats are showing off. Start with (a) baked reflections in
  the texture, or (b) a `Reflector` plane for the tabletop. Ship, then fancy later.

### 5.4 Gaussian splats (optional flex)
- Own splats: film a real object/room with your phone (slow orbit, good light) →
  train in **Postshot** (free, or Luma AI / Polycam) → export `.ply/.splat` →
  render with `@mkkellogg/gaussian-splats-3d` or `gsplat`. Sort worker included.
- Honest take: skip splats for v1. Nobody misses them if the bakes are good.

### 5.5 Wearable-style video panel
- Shoot vertical phone video of your product "worn"/used. Edit, export webm/mp4,
  self-host. Dashed-border frame + HUD overlay = plain HTML/CSS on top of the
  `<video>`, animated in with GSAP. Their cyan UI is DOM, not 3D.

### 5.6 Text
- Headlines: GSAP SplitText per-char reveals (chars slide up with stagger inside
  `overflow: hidden` lines).
- Text INSIDE 3D scenes: `troika-three-text` (MSDF under the hood — same tech as
  their `fonts/msdf/`).
- Fonts: pick open ones (e.g. a grotesque for headlines + mono for labels) from
  Google Fonts/Fontshare, self-host woff2. Never Typekit — that was the piece I
  legally couldn't mirror.

### 5.7 Section transitions + snap
- Full-viewport `position: fixed` canvas behind DOM sections.
- ScrollTrigger pins sections; "SCROLL TO CONTINUE" moments = `snap` config on
  the trigger. Background color/scene crossfades keyed to the same progress value.

### 5.8 Easter eggs (ghost / space-invader sprites)
- Author in Rive (rive.app, free) → export `.riv` → `@rive-app/canvas`, or just
  animated webp/CSS sprites triggered by scroll position. 30-minute job.

## 6. Asset production checklist (the "I don't have their files" list)

- [ ] Product: design YOUR object (or pick your real product) — model it in Blender
- [ ] 1 background plate render per big section
- [ ] Foreground props as RGBA cutout renders
- [ ] 1 glTF with baked tumble animation + camera path
- [ ] 1 room scene glTF, lighting baked to textures
- [ ] Smoke/atmosphere flipbook (optional)
- [ ] 1–2 vertical product videos, self-hosted
- [ ] Open fonts, self-hosted woff2
- [ ] Own wordmark + favicon set
- [ ] Own copy: headlines, feature blurbs, footer jokes

## 7. Suggested build order for the new session

1. Scaffold Vite + Three + GSAP + Lenis; fixed canvas + DOM sections; scroll wired.
2. Hero with placeholder plate (solid color + one cutout) → prove the parallax rig.
3. Scrub rig: cube with baked spin animation scrubbed by scroll → prove 5.2.
4. Replace placeholders with real Blender renders one section at a time.
5. Text reveals + snap + transitions.
6. Video panel section.
7. Polish: postFX (subtle grain/vignette via `postprocessing` pkg), easter eggs.
8. Perf pass: compress textures (webp/avif, `_MOBILE` variants like they did),
   lazy-load below-fold scenes, target <200KB JS before 3D libs.

Verify each stage against the reference frames for *pacing feel* — never pixel-copy.

## 8. Landmines I hit (so you don't)

- Their nav text is SplitText-shattered — text-based click selectors fail.
  Use `#hero / #features` anchor hrefs when testing your own site headlessly.
- Virtual scroll eats synthetic wheel events slowly — headless tests need many
  wheel ticks with waits (or expose a `window.__scrollTo(progress)` debug hook —
  do this, future-you will thank you).
- macOS screen-recording filenames contain a narrow no-break space before "PM" —
  glob them, never type the path.
- Mobile loads DIFFERENT texture variants — test both viewports before calling
  an asset pass done.
