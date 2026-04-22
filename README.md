# Motion Viewer

A browser-based 3D motion visualizer for any robot. Load a URDF with STL meshes and NPZ motion data — everything runs client-side, no server required.

**[Live Demo](https://<your-username>.github.io/motion-viewer/)** (replace with your actual GitHub Pages URL)

## Features

- **Any robot**: Load any URDF + STL mesh bundle via drag-and-drop
- **NPZ motion playback**: Visualize body pose trajectories at arbitrary FPS
- **Zero install**: Pure HTML/JS, runs entirely in the browser (Three.js)
- **Playback controls**: Play/pause, scrub timeline, speed (0.25x–2x), frame stepping
- **Camera follow**: Auto-track the robot root body
- **Trajectory trail**: Toggle a ground-plane trace of the root path
- **Multi-motion**: Load multiple `.npz` files and switch between them with `[` / `]`
- **Keyboard shortcuts**: `Space` play/pause, `←→` step frames, `[]` switch motions

## Usage

1. Open `index.html` in a browser (or visit the hosted site)
2. **Drag a robot folder** containing a `.urdf` file and `.stl` meshes into the window
3. **Drag one or more `.npz` motion files** into the window
4. Use the playback bar and sidebar to control visualization

You can also use the **+ Add .npz** and **+ Add folder** buttons in the sidebar.

## Demo Assets

The `demo/` folder contains a ready-to-use example:

| Asset | Description |
|---|---|
| `demo/unitree_g1/` | Unitree G1 humanoid — URDF + 35 STL meshes |
| `demo/dance1_subject2.npz` | Dance motion clip (131s, 6574 frames @ 50fps, 30 bodies) |

**To try it:**
1. Open the viewer
2. Drag the `demo/unitree_g1/` folder into the window
3. Drag `demo/dance1_subject2.npz` into the window

## NPZ Data Format

Motion files use the NumPy `.npz` format, following the convention from [whole_body_tracking](https://github.com/HybridRobotics/whole_body_tracking). The viewer requires `fps`, `body_pos_w`, and `body_quat_w`; other keys are optional.

```
{
    "fps":            ndarray, shape (1,)              — playback frame rate (e.g. [50])
    "body_pos_w":     ndarray, shape (N, num_bodies, 3) — world-frame body positions
    "body_quat_w":    ndarray, shape (N, num_bodies, 4) — world-frame body orientations (wxyz)
    "joint_pos":      ndarray, shape (N, num_joints)    — joint positions        [optional]
    "joint_vel":      ndarray, shape (N, num_joints)    — joint velocities       [optional]
    "body_lin_vel_w": ndarray, shape (N, num_bodies, 3) — body linear velocities [optional]
    "body_ang_vel_w": ndarray, shape (N, num_bodies, 3) — body angular velocities[optional]
}
```

Where `N` is the number of frames and `num_bodies` matches the number of bodies in the URDF (BFS order, collapsing fixed joints).

## URDF + Mesh Requirements

- Standard URDF format with `<visual>` meshes pointing to `.stl` files
- The viewer resolves meshes by **filename only** (e.g. `package://foo/bar/pelvis.STL` → looks for `pelvis.stl`)
- All `.stl` files must be binary STL format
- Drag the entire folder (URDF + meshes can be in subdirectories)

## Coordinate Convention

- The NPZ data uses a **Z-up** coordinate system (Isaac Sim / IsaacLab convention)
- The viewer automatically converts to Y-up for Three.js rendering
- Quaternions are in **wxyz** order (scalar first)

## Self-Hosting

This is a single HTML file with no build step. To deploy:

### GitHub Pages

1. Fork or clone this repo
2. Go to **Settings → Pages → Source** → select `main` branch
3. Your site will be live at `https://<username>.github.io/motion-viewer/`

### Any Static Server

```bash
# Python
python -m http.server 8000

# Node
npx serve .
```

Or simply open `index.html` directly in a browser — no server needed.

## License

[MIT](LICENSE)
