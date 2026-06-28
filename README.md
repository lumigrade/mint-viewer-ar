# mint-viewer-ar

WebXR **augmented-reality** viewer for LumiGrade **`.mint`** volumetric clips (range-coded 4D gaussian-splat
flipbooks). Same renderer/decoder as the desktop player. Built for **Chrome on Android (ARCore)** — e.g. Galaxy
Tab S9.

**Live: https://lumigrade.github.io/mint-viewer-ar/**

## Pages
- **`index.html`** — simple landing: 4 clips → opens the viewer.
- **`viewer.html?m=<url>`** — the viewer (clean inline turntable preview + WebXR immersive-ar).

## Clips (all hosted on Cloudflare R2 `SEQUENCES/`)
| name | file | notes |
|---|---|---|
| Fashion | `fashion.mint` | **Gracia streaming format — not playable by this viewer** (shows a message). Needs a flipbook re-export. |
| LumiRF8 1350 | `LUMIRF8_1350.mint` | 1350 f, **embedded audio**, graded (`LUMIRF8_1350.grade.json`). 179 MB — heavy for a tablet. |
| HB 180 | `HB_180_Full_HQ.mint` | 39k splats, 180 f, hi-fi. 52 MB. |
| STMTP 390 | `STMTP_390_clean.mint` | 17k splats, 391 f, floaters clamped. Lightest — best for tablet AR. |

## Viewer behaviour
- **Inline preview:** clean plain background, lit, auto-rotating turntable, auto-plays (audio unmutes on first tap).
- **In AR:** point at floor → tap to place · pinch scale · twist turn · tap to move. **Donut** marker shows
  facing + fades after ~3 s. **Cast shadow** (mask→blur→25 % floor) with **Shadow On/Off**. **‹ Back** → landing.
- **Record (AR only):** red iPhone-style button — records the performer + **mic** to the device. The browser
  cannot capture the AR camera passthrough, so the recording shows the performer on a dark backdrop; for the
  full AR view with your room, use the phone's **screen recorder**.
- Grade auto-loads from `<mint>.grade.json` next to the `.mint`.

## Hosting / CORS
All `.mint` (+ `.grade.json`) live on R2 under `SEQUENCES/`. R2 CORS already allows
`https://lumigrade.github.io` (`GET`/`HEAD`) — no change needed.
