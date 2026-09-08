# AR Play Scan — Map Scene Test

A-Frame scene that lays the Bettahalasuru map flat as a textured plane and stands
ten electrical poles on it. This is the geometry check before MindAR goes in.

## Run it

The GLB and the map texture are fetched over HTTP, so opening `index.html`
with `file://` will fail on CORS. Serve the folder:

```bash
# VS Code: right-click index.html -> "Open with Live Server"
# or, from this folder:
npx serve .
python3 -m http.server 8000
```

Then open the printed localhost URL.

**Controls:** drag to look, `W A S D` to move, `Ctrl+Alt+I` for the A-Frame
inspector (the fastest way to drag a pole to a new spot and read off its
position).

## Layout

```
index.html
assets/
  images/map.png                      976 x 826
  models/Electrical_Pole_01.glb       texture embedded, no .mtl/.jpg needed
```

## Coordinate system

The plane is **10 x 8.4631** world units, matching the map's pixel aspect, and
sits flat on XZ with Y up. To convert a pixel on `map.png` into a pole position:

```
x = (px - 488) * 0.010246
z = (py - 413) * 0.010246
y = 0
```

Every pole in `index.html` carries its source pixel coordinate in a comment, so
you can nudge a point without recomputing the set.

The pole model is exactly **1.0 unit tall with its base at y = 0**, so `scale`
is literally the pole's height in world units. All ten are at `0.9`.

## Adding MindAR later

1. Uncomment the MindAR script tag in `<head>`.
2. Put `mindar-image="imageTargetSrc: targets.mind"` on `<a-scene>`.
3. Wrap `#map-root` in `<a-entity mindar-image-target="targetIndex: 0">`.
4. Scale `#map-root` down to about `0.1 0.1 0.1` — MindAR targets are roughly
   1 unit wide, and this map is 10.
5. Delete the `preview-cam` script and the `#rig` entity, and replace them with
   `<a-camera position="0 0 0" look-controls="enabled: false"></a-camera>`.

The scene is already structured so that step 3 is the only structural edit —
nothing outside `#map-root` needs to move.
