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
| A or X | Reset the model in front of you. Hold for 1 second to reset the pose. |
| B or Y | Leave VR and return to the file picker |
| Thumbstick press | Play or pause the animation |
| Thumbstick left / right | Undo / redo the pose (on models that can't be posed: previous / next animation) |

### Posing rigged models

Any model with a skeleton can be posed. The viewer works from the bone hierarchy alone, so bone names and rig conventions don't matter.

| Input | Action |
| --- | --- |
| Trigger + drag | Point at a body part and drag it. The joints above it bend to follow, for example dragging a foot bends the knee and hip. The part stays pinned where you leave it. |
| Both triggers | Pose two parts at once. Grab both shoulders and push one forward, pull the other back, and the torso twists, with the turn spread along the spine. |
| Trigger tap | Pin or unpin a part without moving it. Pinned parts hold still while you pose the rest. |
| Thumbstick up / down | More / fewer joints bend when you drag (1–8, default 3) |

#### With a mouse, in the 2D view

You can also pose on a PC before entering VR, one part at a time:

| Input | Action |
| --- | --- |
| Drag a body part | Pose it. It moves in a plane facing the camera and stays pinned where you leave it. |
| Click a body part | Pin or unpin it |
| Drag empty space | Orbit the camera (scroll to zoom, right-drag to pan) |
| Ctrl+Z / Ctrl+Y | Undo / redo |
| `[` and `]` | Fewer / more joints bend per drag (or the − / + buttons in the panel) |

The two-hand twist needs both controllers, so it's VR only.

While you point, a dot shows the part you'd grab and small dots mark the joints that will bend. Orange dots are pins. Posing pauses any playing animation; pressing play hands control back to the animation. The 2D page also has Undo, Redo, Reset pose and Clear pins buttons.

This is generic inverse kinematics: it doesn't know a knee from an elbow, so a joint can occasionally bend the wrong way. Undo, or drag it back.

## Formats

- **Splats:** `.ply` (3DGS), `.spz`, `.splat`, `.ksplat`, `.sog`, `.zip` (SOG), `.rad`
- **Models:** `.glb`, `.gltf` (embedded), `.obj`, `.fbx`, `.stl`, `.ply` (mesh or points), `.dae`, `.3mf`, `.usdz`

Animated models play automatically. This works for skinned (rigged) and node animations in `.glb`/`.gltf`, `.fbx` and `.dae`. On the 2D page you can pick the animation, pause it and change its speed. Models that store more than 4 bone influences per vertex are reduced to the strongest 4 when they load, since three.js uses 4.

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
