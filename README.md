# Meta Quest 3 – Continuous Flash Suppression (CFS) Panorama

A WebXR prototype of **continuous flash suppression (CFS)** for the Meta Quest 3, built with [three.js](https://threejs.org/) (r160). A rapidly flashing Mondrian mask is shown to one eye while a faint gecko image is shown to the other eye, on top of a 360° panorama seen by both eyes.

## What is CFS?

CFS is a binocular rivalry technique. One eye (the dominant eye) sees a high-contrast, rapidly changing Mondrian pattern. The other eye sees a weak, low-contrast target. The flashing mask usually dominates perception, so the target can stay outside conscious awareness. This lets researchers study how unseen stimuli are processed.

## How it works

- **Panorama:** a 360° equirectangular image (`panorama.jpg`) is shown identically to both eyes, which helps the two eyes fuse the scene.
- **Mask:** a Mondrian of colored rectangles is shown to the dominant eye only (left by default, switchable to right).
- **Target:** the gecko is shown to the other eye only and blinks 2 s on / 2 s off.
- **Head-locked stimuli:** the mask and gecko stay in the same place in the field of view when the head moves.
- **Eye separation:** done with three.js render layers (panorama on both eyes, mask on one eye, gecko on the other).

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole app (HTML, CSS and JavaScript) |
| `panorama.jpg` | Equirectangular 360° background image (you provide this) |
| `gecko.png` or `gecko.jpg` | Target image (you provide this); `gecko.png` is tried first |

If `panorama.jpg` is missing, a blue gradient placeholder is shown. If no gecko image loads, a simple drawn fallback gecko is used.

three.js is loaded from the unpkg CDN, so an internet connection is needed.

## Running it

WebXR needs a secure context (HTTPS, or `localhost`), and the page uses ES modules, so it cannot be opened directly from a `file://` path.

**Option 1: GitHub Pages (easiest for the Quest)**

1. Push `index.html`, `panorama.jpg` and the gecko image to your repository.
2. In the repository, go to **Settings → Pages** and publish from your branch.
3. Open the Pages URL in the **Meta Quest Browser** and click **ENTER VR EXPERIMENT**.

**Option 2: Local server for desktop testing**

```bash
python3 -m http.server 8000
```

Then open `http://localhost:8000` in a desktop browser. You can preview the scene there, but the eye separation only applies inside a VR session.

## Controls

The control panel is on the web page, so set the values **before** entering VR.

| Control | Default | Range |
|---|---|---|
| Gecko opacity | 0.30 | 0.1–1.0 |
| Stimulus scale (gecko + mask) | 1.00× | 0.5–2.5× |
| Gecko blur | 3 px | 0–20 px |
| Background cutoff | 200 | 100–255 |
| Flash rate (mask refresh) | 6 Hz | 1–20 Hz |
| Dominant eye (mask) | Left | Left / Right |

Keyboard: press `[` and `]` to rotate the panorama in 45° steps (useful for choosing the start direction).

## Specifications

| Parameter | Value |
|---|---|
| Platform | Meta Quest 3, WebXR immersive-vr, three.js r160 |
| Headset render scale | 1.5× framebuffer scale |
| Panorama | Equirectangular image on a sphere (radius 50, 64×32 segments), 45° yaw steps with `[` `]` keys |
| Stimulus distance | 2.7 m in front of the viewer |
| Stimulus position | ~2.9° left and ~4.6° below straight ahead |
| Gecko size (scale 1.0×) | 0.18 × 0.11 m, about 3.8° × 2.3° visual angle (scalable 0.5–2.5×) |
| Mask size | 1.8× the gecko, about 6.9° × 4.2° at 1.0× |
| Mask composition | 100–150 rectangles per refresh, made by recursively splitting the largest rectangle (split point 25–75%) |
| Mask colors (probability) | White 40%, black 22%, red 12%, blue 10%, yellow 9%, green 7% |
| Mask flash rate | 6 Hz default (adjustable 1–20 Hz) |
| Gecko opacity | 0.30 default (adjustable 0.1–1.0) |
| Gecko blur | 3 px default (adjustable 0–20 px) |
| Gecko background removal | Luminance cutoff 200 (adjustable 100–255), with a 30-level soft edge |
| Gecko tint | #e8e2d8 |
| Gecko timing | 2 s on / 2 s off, starting visible at session start |
| Dominant-eye mask | Left or right, set by button |
| Gecko texture | Resized to max 1024 px, mipmapped with anisotropic filtering |

## Limitations

- The sliders are on the desktop page, so they must be set before entering VR.
- The mask refreshes once per rendered frame, so the true flash rate is tied to the headset's refresh rate (72/90/120 Hz).
- The default flash rate (6 Hz) is lower than the roughly 10 Hz usually used in CFS studies. Set it to 10 Hz to match the standard.
- The gecko switches on and off abruptly at a fixed opacity rather than ramping its contrast.
- There is no response collection yet (no detection or breakthrough-time task).
- There is no luminance or color calibration of the headset display.

## References

- Tsuchiya, N., & Koch, C. (2005). Continuous flash suppression reduces negative afterimages. *Nature Neuroscience, 8*(8), 1096–1101.
- Korisky, U., Hirschhorn, R., & Mudrik, L. (2019). "Real-life" continuous flash suppression (CFS): CFS with real-world objects using augmented reality goggles. *Behavior Research Methods, 51*(6), 2827–2839. https://doi.org/10.3758/s13428-018-1162-0
- Knotts, J. D., Lau, H., & Peters, M. A. K. (2018). Continuous flash suppression and monocular pattern masking impact subjective awareness similarly. *Attention, Perception, & Psychophysics, 80*, 1974–1987.
