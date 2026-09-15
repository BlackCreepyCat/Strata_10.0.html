<img width="1635" height="1149" alt="image" src="https://github.com/user-attachments/assets/79fa9622-3418-41ab-b073-3c232f96c09b" />

# STRATA, Nodal Terrain Editor

A single-file, browser-based node editor for procedural terrain generation. Build a terrain from noise, erosion, and texturing nodes, preview it in real time in 3D, and export production-ready heightmaps, colormaps, normal maps, and AO maps, no install, no build step, no backend.

<!-- Add a screenshot here, e.g.: ![STRATA screenshot](docs/screenshot.png) -->

## Features

- **Node graph editor**, drag-to-connect procedural pipeline with live thumbnails on every node
- **Real-time 3D preview**, orbit camera, adjustable height scale, adjustable mesh density (256² to 2048², independent of bake resolution), 6 shading modes (Texture, Flat, Clay, Normals, AO, Wire)
- **30 nodes** across inputs, transforms, erosion/geology, texture/detail, and color
- **Resolution up to 8192²** for final export, decoupled from the interactive preview resolution
- **One-click export**, 16-bit heightmap PNG, 8-bit colormap, normal map, and AO map
- **Autosave** to local storage plus manual save/load as `.json` graph files
- **Zero backend**, a single HTML file; open it or serve it statically

## Quick start

STRATA has no build step. Because it loads Three.js as an ES module, browsers won't run it from a bare `file://` URL, so serve the folder locally instead:

```bash
# Python
python -m http.server 8080

# Node
npx serve .
```

Then open `http://localhost:8080/Strata_10.0.html` in a recent Chromium/Firefox/Safari (WebGL2 + ES modules required). An internet connection is needed on first load, since Three.js, the UI font, and icons are pulled from CDNs (unpkg, Google Fonts).

## Node reference

**Inputs**
Constant, FBM Noise, Ridged Noise, Billow Noise, Voronoi, Island Shape, Slope Plane, Heightmap File, Colormap File

**Transform**
Transform, Warp, Blend, Blur, Sharpen, Levels, Invert

**Erosion & Geology**
Hardness Map, Hydraulic Erosion, Thermal Weathering, Flow Map, Terraces, Slope Mask, Snow Cover, Aeolian Erosion, Stream Power, Coastal Erosion

**Texture & Detail**
Strata Bands, Material Layer, Runoff Streaks, Surface Detail, Deposition Map

**Color**
Colorize, Rock Blend, Color Adjust

**Output**
Final Output, combines the height and color chains and drives the 3D preview and all exports

### Hardness Map

Generates a 0 to 1 erosion-resistance mask (random patches, terrain-following rock layers, or both) and feeds an optional **Hardness** input on Hydraulic, Thermal, Aeolian, and Stream Power erosion. `0` means it erodes normally, `1` means it's fully resistant. Used to carve mesas, cliff bands, and waterfalls/knickpoints where a hard layer meets softer rock, instead of uniform erosion everywhere.

## Working with the graph

| Action | Input |
|---|---|
| Add a node | Right-click canvas |
| Connect ports | Drag from a port |
| Open node preview | Double-click a node |
| Select a link | Click it |
| Disconnect | Double-click a link |
| Delete node / link | `Del` |
| Fit view | `F` |
| Save graph | `Ctrl+S` |

## Export

The Export menu (top bar) generates, from the **Final Output** node:

- 16-bit grayscale heightmap PNG
- Colormap PNG
- Tangent-space normal map PNG
- Ambient occlusion PNG

All four share the same resolution as the current bake setting (up to 8192²).

## Tech

Single HTML file, no framework, no bundler. [Three.js](https://threejs.org/) (via import map) for the 3D viewport, [Lucide](https://lucide.dev/) for icons. All terrain math runs on the CPU in `Float32Array`s inside the module script; the 3D viewport renders height, color, normal, and AO as WebGL textures over a heightfield mesh.

## Known limitations

- Tab must stay focused/visible during a heavy rebuild, since background-tab timer throttling can slow down (not break) in-progress builds on very large graphs or high resolutions
- No undo/redo yet
- Single-tile terrains only (no seamless tiling for open-world streaming)

## License

MIT
