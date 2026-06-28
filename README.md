# mint-viewer-ar

WebXR **augmented-reality** viewer that plays LumiGrade **`.mint`** volumetric clips (range-coded 4D
gaussian-splat flipbooks) **directly** — the same renderer/decoder as the desktop `view.html`, so AR shows
the exact same animation. Built for **Chrome on Android (ARCore)** — e.g. Galaxy Tab S9.

**Live: https://lumigrade.github.io/mint-viewer-ar/**

## What it does
- Streams a `.mint` from Cloudflare R2, range-decodes it in the browser (rANS → per-frame splats), and
  plays the flipbook.
- **Desktop:** orbit preview in the HDRi street scene (same as the studio player) + a **View in AR**
  button that appears only on AR-capable devices.
- **AR (Android):** tap **View in AR** → point at the floor (a ring appears) → **tap to place** the
  performer at life size on the real floor → **pinch** to scale, **two-finger twist** to turn, **tap** to
  reposition. Camera passthrough is the background; live color-grade + de-spike from the studio carry over.

## How it's built
Single static page — no build step. It is `view.html` (the `.mint` WebGL2 player: rANS decoder, splat
shader with de-spike + grade, depth sort, flipbook playback) **plus** a WebXR `immersive-ar` module
(hit-test reticle, tap-to-place, pinch/twist, an XR render loop that drives the *same* splat shader with
the device camera pose). No Three.js, no Spark, no `.spz` — it plays `.mint` natively.

## Data hosting (Cloudflare R2)
`.mint` files are **not** committed (too large; reusable). They live on R2 (CORS must allow
`https://lumigrade.github.io`). The viewer's default is `R2_BASE + "STMTP_390.mint"` (edit `R2_BASE` near
the top of the `load()` function in `index.html`). Override per-load with **`?m=<name-or-full-URL>`**
(a bare name resolves to `/MINT_DATA/<name>.mint` for local dev against `serve.py`).

Current default: **`STMTP_390.mint`** (391 frames, 25 fps, ~17k splats/frame — fits the Tab S9 budget).
Upload it to the bucket's `SEQUENCES/` prefix so it resolves at
`https://pub-74912f96f8a84c0db42d38800266b931.r2.dev/SEQUENCES/STMTP_390.mint`.

> Note on scale: the AR viewer **pre-loads all frames to GPU**, so keep clips light for mobile (STMTP_390
> ≈ 0.4 GB). Very long/dense clips (e.g. LUMIRF8_1350 ≈ 2.75 GB) need windowed streaming and are not
> suitable for a tablet pre-load.

## Files
```
index.html    the AR viewer (mint player + WebXR immersive-ar)
.nojekyll     serve assets verbatim on GitHub Pages
README.md
```
