# Quest splats and 3D models

A single-page WebXR viewer for Meta Quest 3. Load a Gaussian splat or a 3D model from the headset's storage, and it opens in VR. You move the model by grabbing the space around it with the controllers.

Built with [three.js](https://threejs.org) and [Spark](https://sparkjs.dev). Spark builds a level-of-detail version of the splat when it loads, so files up to about 200 MB stay smooth on Quest 3.

## Use it

1. Open the GitHub Pages URL for this repo in the Quest browser.
2. Tap **Choose a file…** and pick a splat or model, for example from Downloads.
3. When it finishes loading, the viewer tries to enter VR on its own. If the browser asks for a tap first, press **Enter VR**.

## Controls

| Input | Action |
| --- | --- |
| Grip (one hand) | Grab space and push, pull or slide the model |
| Grip (both hands) | Spread apart to zoom in, bring together to zoom out, turn like a steering wheel to rotate |
| A or X | Reset the model in front of you |
| B or Y | Leave VR and return to the file picker |

## Formats

- **Splats:** `.ply` (3DGS), `.spz`, `.splat`, `.ksplat`, `.sog`, `.zip` (SOG), `.rad`
- **Models:** `.glb`, `.gltf` (embedded), `.obj`, `.fbx`, `.stl`, `.ply` (mesh or points), `.dae`, `.3mf`, `.usdz`

Files must be self-contained. External textures and `.mtl` files aren't loaded, so use GLB for textured models.

## Settings

The settings panel has these options:

- **Display:** VR or passthrough
- **Splat quality:** trades the splat budget and render resolution for frame rate
- **Flip splats upright:** most 3DGS training output is upside down
- **One-hand grab also rotates**
- **Keep model level:** two-hand rotation only spins around the vertical axis
- **Show floor grid**

## Running locally

From this folder, run:

```
python -m http.server 8000
adb reverse tcp:8000 tcp:8000
```

Then open `http://localhost:8000` in the Quest browser. WebXR works on `localhost` without HTTPS. The page loads three.js and Spark from a CDN, so the headset needs internet access.
