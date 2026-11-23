# Onion Skin Aligner

A single-page, browser-only tool for aligning an image over a calibrated background and reading off points of interest in global (real-world) coordinates.

- Load a **background image** and calibrate it to a global coordinate system using two reference points.
- Load an **overlay image**, shown semi-transparent for onion skinning.
- Define **points of interest** on the overlay.
- Move, scale and rotate the overlay on top of the background; the global coordinates of every point update live.
- Export points as CSV or copy them to the clipboard.
- Save and reopen projects as plain-text `.osa` files.

Everything runs client-side. No build step, no dependencies, no server code, no data leaves the browser.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole application (HTML, CSS and JavaScript in one file) |
| `example.osa` | Sample project file showing the `.osa` format |
| `README.md` | This file |

## Deploying

It's a static site: serve `index.html` from any static host (GitHub Pages, Netlify, Cloudflare Pages, S3, etc.). No configuration is needed.

For GitHub Pages: push these files to the root of a repository, then enable Pages from the `main` branch, root folder.

To run locally, just open `index.html` in a modern browser (Chrome, Edge, Firefox, Safari).

## Using it

1. **Images**: choose a background file and an overlay file. Use the opacity slider to set how see-through the overlay is.
2. **Calibrate**: switch to *Calibrate* mode, click reference point A then B on the background, and enter their global coordinates. Tick *Global Y increases upward* for map/graph-style coordinates. Calibration is a similarity transform (uniform scale, rotation, translation, optional Y-flip).
3. **Add points**: switch to *Add points* mode and click on the overlay. Points are stored in overlay-local pixels, so they move with the overlay.
4. **Move overlay**: in *Move overlay* mode, drag to move. Scale and rotation sliders pivot about the overlay image's centre.

### Controls

| Action | Input |
|---|---|
| Pan view | Drag in *Pan view* mode |
| Zoom view | Scroll wheel (any mode) |
| Move overlay | Drag in *Move overlay* mode |
| Scale overlay | Scale slider, or Shift + scroll in *Move overlay* mode |
| Rotate overlay | Rotation slider, or Alt + scroll in *Move overlay* mode |
| Nudge overlay | Arrow keys (hold Shift for fine steps) |

## `.osa` project format (version 1)

Plain ASCII text in an INI-style layout. Lines starting with `#` or `;` are comments. Images are **not** embedded; only their file names are recorded, and the app asks you to reload them after opening a project.

```
# Onion Skin Aligner project
version = 1

[images]
background = <file name>
overlay = <file name>

[calibration]
refA_px = <x>, <y>          # reference point A in background pixels
refB_px = <x>, <y>          # reference point B in background pixels
A_global = <x>, <y>         # global coordinates of A
B_global = <x>, <y>         # global coordinates of B
flip_y = 0 | 1              # 1 = global Y increases upward

[overlay_transform]
x = <px>                    # overlay top-left position, background pixels
y = <px>
scale = <factor>
rotation_deg = <degrees>
opacity = <5-100>

[points]
# overlay_local_x, overlay_local_y, name
<x>, <y>, <name>
```

Point names may contain commas; everything after the second comma is treated as the name.

## Browser notes

- *Copy* uses the Clipboard API, which requires the page to be served over HTTPS (or `localhost`). Opening the file directly from disk may block it in some browsers; CSV download still works.
- Alt + scroll may be intercepted by some operating systems or window managers; use the rotation slider instead.
