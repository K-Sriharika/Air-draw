# ✦ Air Draw — Gesture Canvas

[![MIT License](https://img.shields.io/badge/License-MIT-7c4dff.svg)](LICENSE)
[![MediaPipe](https://img.shields.io/badge/MediaPipe-Hands%200.4-00f5ff.svg)](https://mediapipe.dev)
[![Zero Dependencies](https://img.shields.io/badge/Dependencies-Zero-39ff14.svg)]()
[![Single File](https://img.shields.io/badge/Delivery-Single%20HTML-ff00ff.svg)]()
[![GitHub Pages](https://img.shields.io/badge/Deploy-GitHub%20Pages-0a0a0f.svg)](https://pages.github.com)

> Draw in the air with your bare hand. Watch your strokes become living light.  
> No installs. No server. No build step. Just open and create.

---

<!-- Replace with your actual demo GIF — record with LICEcap (Win) or Kap (Mac) -->
![Air Draw Demo](assets/demo.gif)

---

## ✨ What It Does

Air Draw uses your webcam and **MediaPipe Hands** to track your hand in real-time at ~30fps. Your index finger becomes a paintbrush. Every stroke you draw **stays animated** — glowing, pulsing, and flowing with a traveling energy orb — turning your canvas into a living artwork.

---

## 🤙 Gesture Reference

| Gesture | Action | How |
|:---:|---|---|
| ☝️ | **Draw** | Index finger extended, others curled |
| 🖐️ | **Erase** | All five fingers open/extended |
| ✊ | **Pause** | Closed fist — lifts the pen |
| 🤏 | **Resize brush** | Pinch thumb + index together; spread to enlarge |

---

## ⚡ Features

- **Real-time hand tracking** — MediaPipe Hands runs entirely in-browser (WASM)  
- **Animated living strokes** — drawn lines continuously pulse, glow, and carry a traveling light orb  
- **Particle trail** — sparkles burst from the drawing tip while you draw  
- **Smooth bezier curves** — quadratic interpolation between hand positions prevents jagged lines  
- **Pinch-to-resize** — spread/close your pinch to control brush diameter (3px → 50px)  
- **6 neon color presets** + custom color picker  
- **Eraser with stroke splitting** — erases at point-level, cleanly splitting strokes  
- **Undo / Clear / Save PNG**  
- **FPS counter** — signals performance awareness  
- **Gesture HUD** — real-time feedback on what gesture is detected  
- **Zero dependencies** — single `.html` file, works offline after first load  

---

## 🚀 Live Demo

👉 **[air-draw.yourusername.github.io](https://yourusername.github.io/air-draw)**  
*(Replace with your GitHub Pages URL after deploying)*

---

## 🛠 Run Locally

No build step, no npm install:

```bash
# 1. Clone the repo
git clone https://github.com/yourusername/air-draw.git
cd air-draw

# 2a. Open directly (Chrome/Edge — works for most users)
open index.html

# 2b. Or serve locally (required if direct open blocks camera)
npx serve .
# → visit http://localhost:3000
```

> **Note:** Webcam access requires either `localhost` or `https://`. If your browser blocks camera on `file://`, use `npx serve .` or deploy to GitHub Pages.

---

## 📦 Deploy to GitHub Pages (2 minutes)

### Option A — GitHub UI

1. Push your code to a GitHub repo
2. Go to **Settings → Pages**
3. Under *Branch*, select `main` / `root`
4. Click **Save**
5. Your app is live at `https://yourusername.github.io/repo-name`

### Option B — GitHub Actions (auto-deploy on push)

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pages: write
      id-token: write
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - uses: actions/checkout@v4
      - uses: actions/configure-pages@v4
      - uses: actions/upload-pages-artifact@v3
        with:
          path: '.'
      - id: deployment
        uses: actions/deploy-pages@v4
```

Every `git push` will auto-deploy. ✓

---

## 🧠 How It Works

```
Webcam → MediaPipe Hands WASM → 21 hand landmarks (x, y, z)
                                       ↓
                            Gesture classifier
                         (finger extension logic)
                                       ↓
              ┌─────────────────────────────────────┐
              │        Canvas Rendering Loop         │
              │   requestAnimationFrame @ ~60fps     │
              │                                      │
              │  For each stored stroke:             │
              │  ├── Outer glow (shadowBlur + pulse) │
              │  ├── Neon core line                  │
              │  ├── White fiber-optic center        │
              │  └── Traveling energy orb            │
              │                                      │
              │  Particle system (draw sparks)       │
              │  Cursor renderer (gesture-aware)     │
              └─────────────────────────────────────┘
```

### Key technical decisions

| Choice | Reason |
|---|---|
| Single HTML file | Zero friction — deploy anywhere, share as one link |
| MediaPipe WASM | GPU-accelerated hand tracking, no Python/server needed |
| Full stroke re-render per frame | Enables per-frame animation on every drawn line |
| Quadratic bezier smoothing | Eliminates jagged artifacts from noisy hand tracking |
| Point-level erasing with stroke splitting | Natural erase UX — partial strokes survive |
| Golden-ratio phase offset per stroke | Prevents all animation orbs from syncing identically |

---

## 📁 Repository Structure

```
air-draw/
├── index.html          ← entire application (one file)
├── README.md           ← this file
├── LICENSE             ← MIT
└── assets/
    ├── demo.gif        ← record with LICEcap / Kap / ScreenToGif
    └── preview.png     ← static screenshot for README / og:image
```

---

## 🎥 Recording Your Demo GIF

A good demo GIF is the most important part of a portfolio project.

**Mac:** [Kap](https://getkap.co/) — free, exports GIF/MP4  
**Windows:** [ScreenToGif](https://www.screentogif.com/) — free, frame-by-frame editor  
**Cross-platform:** [LICEcap](https://www.cockos.com/licecap/) — ultra-lightweight

**Tips:**
- Keep it under 15 seconds
- Show: draw → pause → erase → pinch resize → save
- Good lighting on your hand helps MediaPipe track better
- Export at 600×400 or smaller to keep the GIF under 5MB

---

## ⌨️ Keyboard Shortcuts

| Key | Action |
|---|---|
| `Ctrl/Cmd + Z` | Undo last stroke |
| `Ctrl/Cmd + S` | Save canvas as PNG |
| `Escape` | Clear all |

---

## 🔧 Customization

Open `index.html` and find the `CONFIGURATION` section at the top of the `<script>` tag:

```javascript
const PALETTE = [
  '#00f5ff',  // cyan — swap any hex you like
  '#ff00ff',  // magenta
  // ...
];

const MIN_BRUSH    = 3;    // minimum brush diameter (px)
const MAX_BRUSH    = 50;   // maximum brush diameter (px)
const ERASER_RAD   = 55;   // eraser circle radius (px)
const PINCH_THRESH = 0.10; // pinch sensitivity (lower = more sensitive)
const MAX_STROKES  = 50;   // canvas memory cap
const ORB_SPEED    = 0.25; // energy orb travel speed
```

---

## 🤝 Contributing

Issues and PRs welcome. Some ideas for extensions:

- [ ] Multi-hand support (two-handed drawing)
- [ ] Stroke export as SVG
- [ ] Replay / time-lapse playback of the canvas
- [ ] Collaborative canvas via WebRTC
- [ ] Voice commands for color switching

---

## 📜 License

MIT — use it, fork it, build on it, ship it.

---

<div align="center">
  <sub>Built with MediaPipe · Canvas API · Pure JavaScript</sub><br>
  <sub>No frameworks. No bundler. No excuses.</sub>
</div>
