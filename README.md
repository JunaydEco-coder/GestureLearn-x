# GestureLearn X

**Hand-tracking based 3D interactive learning & design system — fully browser-based, touchless, zero-install.**

GestureLearn X turns a standard webcam into a touchless controller for a 3D scene. Using [MediaPipe Hands](https://developers.google.com/mediapipe) for real-time 21-landmark hand tracking and [Three.js](https://threejs.org/) for WebGL rendering, it lets you explore a labelled 3D anatomical model with an AI tutor, build and manipulate 3D shapes with your hands, and draw in the air — all with no mouse, no keyboard, and no backend server.

> IT4609 – Mini Project · Department of Information Technology · St. Joseph's Institute of Technology (Anna University).

---

## Features

### Learning Mode
- A 3D heart built from six selectable, labelled components.
- **Point** at any part and an **AI Tutor panel** types out a conversational explanation, then lists key facts.
- **Thumbs Up** makes the tutor read the description **aloud** (browser speech synthesis — fully offline).
- **Victory** tours to the next part automatically.
- A glowing reticle shows exactly where your finger is pointing.

### Design Mode
- Create primitives: **cube, sphere, cylinder, cone, torus, and line**.
- **Point** to select, **Fist** to grab and move an object smoothly in 3D, **Open Palm** to rotate it, **Pinch** to scale it.
- Duplicate, delete (button or the `Delete` key), and clear-all controls.
- Live selection box and on-screen transform readout.

### Air Draw Mode
- Draw glowing 3D strokes in space with your **index finger**.
- Strokes are placed on a camera-facing plane — orbit the scene to see your drawing in true 3D.
- Four brush colours, undo-last-stroke, and clear-canvas.

### Across the whole app
- **7-gesture vocabulary** with a 3-frame stability window to prevent misfires.
- Exponential smoothing on landmarks plus target-based easing on every transform for jitter-free motion.
- Live FPS counter and capped pixel ratio to keep performance smooth during a live demo.

---

## Gesture vocabulary

| Gesture | Learning Mode | Design Mode | Air Draw Mode |
|---------|---------------|-------------|---------------|
| **Point**     | Select a part      | Select an object   | Draw         |
| **Fist**      | Rotate the model   | Grab & move object | —            |
| **Pinch**     | Zoom in / out      | Scale object       | —            |
| **Open Palm** | Reset view         | Rotate object      | —            |
| **Victory**   | Next part          | Duplicate object   | Next colour  |
| **Thumbs Up** | Read aloud         | Reset object       | Undo stroke  |
| **Three**     | Toggle auto-rotate | Toggle auto-rotate | —            |

The mouse still works as a fallback: drag to orbit the camera, click to select an object in Design Mode.

---

## Getting started

This is a **single-file, zero-build** web app. There is nothing to install.

### Option 1 — just open it
Double-click `index.html` to open it in **Google Chrome** (recommended). Click **Start camera** and allow webcam access.

> Browsers may block the webcam on pages opened via `file://`. If the camera does not start, use Option 2.

### Option 2 — run a local server (recommended)
Any static server works:

```bash
# Python 3
python -m http.server 5173

# or Node
npx serve .
```

Then open http://localhost:5173

### Option 3 — GitHub Pages (free live demo)
Push this repo to GitHub, then enable **Settings -> Pages -> Deploy from branch -> main / root**. Because the entry file is `index.html`, your app goes live at `https://<your-username>.github.io/<repo-name>/`.

---

## Tech stack

| Technology | Purpose |
|------------|---------|
| MediaPipe Hands | Real-time 21-landmark hand tracking (WASM, loaded from CDN) |
| Three.js (r160) | WebGL 3D rendering, scene graph, raycasting |
| JavaScript (ES Modules) | All application logic — no framework, no bundler |
| Web Speech API | Offline text-to-speech for the AI tutor |

No backend, no external AI API, no build step. Everything runs client-side in the browser.

---

## Project structure

```
GestureLearnX/
├── index.html      # The entire app: HTML + CSS + JS (Three.js scene, gesture classifier, all 3 modes)
├── README.md       # This file
├── LICENSE         # MIT
├── package.json    # Optional convenience dev-server script
└── .gitignore
```

The whole application lives inside `index.html`. Open it in an editor to read or modify the source — the logic is in the `<script type="module">` block near the bottom.

---

## How it works

```
Webcam -> MediaPipe Hands -> Gesture classifier -> Interaction controller -> Three.js scene
                             (21 landmarks ->         (gesture -> camera /        + AI info panel
                              threshold rules)         object transforms)
```

1. **Sensing** — each webcam frame is sent to MediaPipe Hands, which returns 21 hand landmarks.
2. **Smoothing** — landmarks pass through an exponential moving-average filter to reduce jitter.
3. **Classification** — a deterministic, threshold-based classifier maps finger states to one of seven gestures, confirmed over a short stability window.
4. **Interaction** — gestures update *target* transforms; the render loop eases objects toward those targets each frame for smooth motion.
5. **Knowledge layer** — selecting a part queries a local, curated knowledge base and renders it as a typed AI-tutor response.

---

## Browser support
- **Recommended:** Google Chrome 90+ (best MediaPipe WASM + WebRTC support).
- Also works on recent Edge and Firefox.
- Requires a webcam and a GPU with WebGL 2.0.

---

## Known limitations
- The heart in Learning Mode is a stylised model built from primitives (a clean stand-in for a GLTF asset).
- RGB-only hand tracking is less reliable in low light or against a busy background.
- Air-draw and design sessions are not persisted between page reloads.

---

## Future work
- Load a real GLTF anatomical model and add more subjects (brain, engine, etc.).
- Two-handed (bimanual) interaction.
- Grid snapping and alignment guides in Design Mode.
- Save / export designs and air-draw sketches.
- Optional live language-model tutor in place of the static knowledge base.

---

## Authors
- **Mohammed Junayd Ali M** — Reg No. 312423205133
- **Nova Antony Rohan E** — Reg No. 312423205156

Department of Information Technology, St. Joseph's Institute of Technology, Chennai.

---

## License
Released under the [MIT License](LICENSE).

## Acknowledgements
Built on the open-source work of the MediaPipe and Three.js communities.
