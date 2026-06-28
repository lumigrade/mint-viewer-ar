# mint-viewer-ar

WebXR **augmented-reality** viewer that plays LumiGrade **`.mint`** volumetric clips (range-coded 4D
gaussian-splat flipbooks) directly — same renderer/decoder as the desktop player. Built for **Chrome on
Android (ARCore)** — e.g. Galaxy Tab S9.

**Live: https://lumigrade.github.io/mint-viewer-ar/**

## Pages
- **`index.html`** — landing page: pick one of three clips → opens the viewer.
- **`viewer.html?m=<name-or-url>`** — the AR viewer (mint player + WebXR immersive-ar).

## The three clips
| clip | `?m=` | host | notes |
|---|---|---|---|
| Performer (clean) | `./STMTP_390.mint` | Pages | 17k splats, 391 f, floaters removed. Lightest — best for tablet AR. |
| Dancer (HQ) | `./HB_180_Full_HQ.mint` | Pages | 39k splats, 180 f, hi-fi. Heavier. |
| Performance + audio | R2 URL | **R2** | 36k splats, 1350 f (54 s), **graded + synced audio**, ~179 MB. Desktop/high-RAM; heavy for a tablet pre-load. |

## In the viewer
- **Place:** point at the floor → tap. **Pinch** scale · **two-finger twist** turn · **tap** to move.
- **Donut** base-marker shows position + facing, fades after ~3 s of no touch.
- **Cast shadow** (offscreen silhouette mask → blur → 25 % floor darken); **Shadow On/Off** toggle (top-left).
- **Red record button** — records the canvas + **mic** to the device (Downloads/Gallery). NOTE: in *immersive
  AR* the browser cannot capture the camera passthrough — it records the rendered performer only; for the full
  AR view use the phone's **screen recorder**. Inline/desktop records the whole scene + mic.
- **G** = color-grade panel; a saved `<name>.grade.json` next to the `.mint` auto-applies (the graded look).

## Data hosting (Cloudflare R2)
`.mint` ≤ 100 MB live in this repo (Pages). Bigger clips live on **R2** (CORS already allows
`https://lumigrade.github.io`), referenced by a full URL in `?m=`. The grade is fetched from the **same
location** as the `.mint` (`<mint>.grade.json`), so upload it alongside.

### To enable the "Performance + audio" clip
Upload to the bucket `SEQUENCES/` prefix (so they resolve at `…r2.dev/SEQUENCES/…`):
- `LUMIRF8_1350.mint`
- `LUMIRF8_1350.grade.json`  ← the graded look (black/white levels, cool temp, de-spike)
