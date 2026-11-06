# Butterfly Life Cycle

An interactive, single-file web experience that follows three monarch butterflies from egg to adult. Drag a scrubber and watch each stage develop on a floating pixel-art diorama.

Everything lives in one `index.html`. There is no build step, no bundler and no dependencies to install. Three.js is loaded from a CDN.

## Features

- **Seven stages**, driven by one global timeline `t` from 0 to 1:

  | Stage | Range |
  |---|---|
  | Egg | 0–12% |
  | Hatching | 12–25% |
  | Caterpillar | 25–50% |
  | J-hang | 50–58% |
  | Chrysalis | 58–80% |
  | Emerging | 80–90% |
  | Butterfly | 90–100% (flaps, lifts off and hovers) |

- **Three monarchs** on leaves and twigs, arranged in a triangle. Each develops on a slightly offset schedule and at a slightly different size.
- **Fine pixel-art look.** Everything is procedurally painted, with no image assets. It uses dithered shading, textured materials, pixel-grid outlines and a soft floating shadow.
- **Apple "Today"-style tick scrubber** with a thumb, a bold stage title and a "Monarch · Day N" subtitle.
- **Slow turntable rotation** of the scene.
- **Eased transitions**, running at 60 fps.
- **Accessible:** mouse, touch and keyboard control, ARIA slider semantics, and `prefers-reduced-motion` support.
- **Responsive** layout for desktop and phone.

## Run it

1. Download or clone the repo.
2. Open `index.html` in a modern browser. WebGL and an internet connection are needed, because Three.js (r128) is loaded from cdnjs.

To serve it locally instead:

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## Controls

| Input | Action |
|---|---|
| Click / drag the scrubber | Jump to any point in the life cycle |
| ← / → | Step through time |
| Home / End | Jump to the start / end |

## Customize

All tunable values are in the `CONFIG` object near the top of `index.html`. Common ones:

| Constant | What it does |
|---|---|
| `PIXEL_SCALE` | Size of one art pixel (smaller = finer pixels) |
| `DITHER_LEVELS` / `DITHER_STRENGTH` | Shading steps and dither amount |
| `ROTATION_SPEED` | Turntable speed in rad/s (0 to disable) |
| `BUTTERFLY_COUNT` | Number of monarchs |
| `UNIT_RADIUS` | Spacing of the monarchs |
| `UNIT_TIME_OFFSET_MAX` | How far apart the monarchs' timelines are |
| `UNIT_SIZE_VARIATION` | Size variation between monarchs |
| `TOTAL_DAYS` | The day count shown in the subtitle |
| `CAMERA_ELEVATION_DEG`, `VIEW_SIZE` | Camera angle and zoom |
| `SHOW_GROUND` / `PROPS` | Toggle the floating slab and its flowers, pebbles and mushrooms |

## How it works

- **Rendering:** Three.js draws into a low-resolution render target, and a post-process pass adds depth-based outlines. The canvas is then scaled up with an exact whole number of device pixels per art pixel, so pixels stay crisp at any DPR.
- **Shading:** lighting uses an ordered Bayer-dither quantisation. Textures are painted at load time from colour ramps and noise.
- **Creatures:** the caterpillar is a tapered tube bent along a centre-line, crawling and then swinging into the J-hang. The chrysalis forms, darkens, shows the wing pattern, splits and leaves a husk. The butterfly's wings crumple while wet, then expand, dry and flap.
- **Timeline:** one `t` value drives every unit. Each monarch derives its own progress from it.

## Tech

HTML, CSS and vanilla JavaScript, plus [Three.js](https://threejs.org/) r128.
