<div align="center">

# Relief

**Turn a greyscale depth map into a 3D relief on a solid board — live in your browser.**

Adjust height, size, material and lighting, then export an **STL** for CNC carving or 3D printing, or save a **PNG** render.

### [→ Open the live demo](https://snazzygaz.github.io/relief/)

[![Live Demo](https://img.shields.io/badge/live-demo-e7ab4f?style=flat-square)](https://snazzygaz.github.io/relief/)
[![License: MIT](https://img.shields.io/badge/licence-MIT-4c9a8f?style=flat-square)](LICENSE)
![No build](https://img.shields.io/badge/build-none-6f6556?style=flat-square)
![Offline](https://img.shields.io/badge/works-offline-6f6556?style=flat-square)
![three.js](https://img.shields.io/badge/three.js-r128-6f6556?style=flat-square)

</div>

Everything runs client-side in a single HTML file. No build step, no server, nothing to install — Three.js is inlined, so it works fully offline.

<!-- Optional: drop a screenshot in the repo and uncomment -->
<!-- <div align="center"><img src="screenshot.png" alt="Relief screenshot" width="820"></div> -->

---

## ✦ Features

- Load any greyscale image as a depth map (light = high, dark = low), or start from a generated sample.
- Solid, watertight board mesh (displaced top surface + sides + flat bottom) — not a floating sheet, so exports are printable.
- Materials: **Wood**, **Stone**, **Steel** (with reflections) and **Plain** (pick any colour). Matte / Satin / Gloss finish.
- Draggable light control: set direction and elevation from grazing to straight overhead.
- Real-time controls for relief height, smoothing, detail, board size and thickness.
- **STL export** (CNC / 3D printing) and **PNG** render export.
- Render-on-demand — idle uses almost no GPU.

---

## ✦ Using it

1. Click **Load** to choose a greyscale image, drag one onto the viewport, or click **Sample** to generate one.
2. Shape the relief with the sliders on the right.
3. Orbit to inspect, then **Export STL** or save an image.

### Viewport controls

| Action | Control |
| --- | --- |
| Rotate | Left-drag |
| Zoom | Mouse wheel / pinch |
| Pan | Right-drag, middle-drag, or two-finger drag |
| Reset view | **Reset view** button |

### Panel

| Section | What it does |
| --- | --- |
| **Relief** | Height (relief depth), Smoothing (blurs the map), Detail (mesh resolution), Invert depth |
| **Board** | Size (long edge) and Thickness |
| **Material** | Wood / Stone / Steel / Plain, with Tone/Grain (or a Colour picker for Plain) and Matte/Satin/Gloss finish |
| **Light** | Drag the pad — horizontal = direction, vertical = elevation (top = overhead). Brightness below |
| **View** | Auto-spin toggle and Reset view |

### Depth map tips

- Use a **greyscale** image. White rises, black sinks.
- Higher **Detail** gives crisper geometry but a larger STL and heavier preview; drop it while adjusting, raise it for the final export.
- A little **Smoothing** removes stair-stepping from low-quality or heavily compressed maps.
- **Invert depth** if your map uses the opposite convention (dark = high).

---

## ✦ Hosting on GitHub Pages

The repo just needs `index.html` at the root.

1. Create a repository and add `index.html` (provided).
2. Push to GitHub:
   ```bash
   git init
   git add index.html README.md LICENSE
   git commit -m "Relief: depth map to 3D relief"
   git branch -M main
   git remote add origin https://github.com/snazzygaz/relief.git
   git push -u origin main
   ```
3. On GitHub: **Settings → Pages → Build and deployment**. Set **Source** to *Deploy from a branch*, branch `main`, folder `/ (root)`, and Save.
4. Wait a minute, then visit **https://snazzygaz.github.io/relief/**.

There's nothing to build.

---

## ✦ Notes and limitations

- **Offline:** the only "external" string in the file is an XML namespace inside Three.js, not a network call. It runs with no connection.
- **STL units** are nominal millimetres and export at the slider values shown. There's no calibration to the source image's real-world size, so scale/verify in your CAD or slicer before cutting.
- At maximum **Detail** the STL can be large (tens of MB) and rebuilds take a moment on slower GPUs.
- Rendering uses WebGL; any current desktop or mobile browser works.

---

## ✦ How it works

- The depth image is downsampled to a height grid; a solid board mesh is built once per resolution and its vertex buffers are updated in place when you change height, size or thickness (no per-frame rebuilds).
- Surface normals are derived analytically from the height gradient, so lighting reflects the carved detail.
- Wood / stone / steel textures are generated procedurally from a value-noise lattice.
- STL export walks the mesh triangles and writes a binary STL; PNG export grabs the WebGL canvas.

---

## ✦ Licence

Released under the [MIT Licence](LICENSE).

Bundles [Three.js](https://threejs.org) (r128) — © Three.js authors, MIT Licence.
