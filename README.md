# VRK — Kinect Point Cloud Demos

VRK is a collection of browser-based Kinect/depth-video point-cloud experiments built with Three.js and WebGL.

The optimized `index2` demo is now the repository home page at [`index.html`](index.html). All earlier experiments remain available under [`pages/`](pages/), with shared media and styles centralized under [`assets/`](assets/).

## Run locally

Serve the repository root over HTTP:

```sh
python3 -m http.server 8080
```

Then open `http://localhost:8080/`. Opening the HTML files directly is not recommended because browsers may block JavaScript modules or video textures on `file://` URLs.

## Available pages

| Page | Local URL | Description | VR |
| --- | --- | --- | --- |
| [Home](index.html) | `/` | The former `index2` demo. Optimized opaque point cloud with angled clipping, adaptive quality, inertial camera movement, organic drift, and settings toggled with Space. | No |
| [Original](pages/original/index.html) | `/pages/original/` | The original `index.html` experiment using the full 640×480 `2.mp4` recording and desktop mouse camera. | No |
| [Demo 1](pages/1/index.html) | `/pages/1/` | High-density 1600×1056 point grid using `output.mp4`, with a 90° X-axis rotation and scale control. | Yes |
| [Demo A](pages/a/index.html) | `/pages/a/` | `output.mp4` rendered with large, translucent additive points. | Yes |
| [Demo B](pages/b/index.html) | `/pages/b/` | Opaque variation of Demo A. | Yes |
| [Demo C](pages/c/index.html) | `/pages/c/` | `kinect2.mp4` with a 90° X-axis rotation and uniform scale control. | Yes |
| [Claude](pages/claude/index.html) | `/pages/claude/` | VR-oriented variant with controller models, FPS reporting, and extended transform controls. | Yes |

The legacy VR pages load Three.js support modules from jsDelivr and therefore require an internet connection. WebXR normally also requires HTTPS or a localhost origin.

## Home-page controls

The home-page settings are hidden initially:

- Press **Space** to reveal them.
- Press **Space** again or **Escape** to hide them.
- Hold **<**/**,** or **>**/**.** to pause playback and jump to a different, non-linear moment once per second. Release the key to resume from the displayed moment.
- **Performance** changes point-grid density and adaptive display resolution.
- **Image effects** tunes the lightweight bloom pass and subtle blue grade.
- **Depth mapping** changes brightness-to-depth conversion, point size, and Z offset.
- **Visibility clipping** controls the close plane, far plane, far-cut angle, and top crop in source-video pixels.
- **Mesh transform** controls mesh position and X-axis rotation.

The home page uses a jerk-smoothed damped spring for mouse tracking and blends a slow `200 × 120 px` organic drift into the same camera target. Drift and mouse parallax are disabled when the operating system requests reduced motion.

The point-cloud shader masks 10 source-video pixels at the top by default to remove source noise without modifying the video asset. Tune **Top crop (px)** under **Visibility clipping**.

The home page uses a single fullscreen post-processing pass for a restrained bloom and cool-blue grade. Timeline scrubbing samples shuffled regions across the entire video, holds each sought frame while the key is down, and resumes normal playback immediately on release.

## Project structure

```text
.
├── index.html                 # optimized index2 home page
├── style.css                  # home-page styles
├── metadata.json              # home-page publication metadata
├── assets/
│   ├── styles/
│   │   ├── main.css           # shared legacy demo styles
│   │   └── text-effects.css   # preserved text/filter experiment
│   ├── vendor/                # local Three.js and lil-gui modules/licenses
│   └── videos/                # shared depth-video sources
└── pages/
    ├── original/index.html
    ├── 1/index.html
    ├── a/index.html
    ├── b/index.html
    ├── c/index.html
    └── claude/index.html
```

## Video assets

| File | Used by |
| --- | --- |
| `assets/videos/2-optimized-trimmed.mp4` | Home; begins 50 seconds into the optimized source |
| `assets/videos/2-optimized.mp4` | Preserved full-length optimized source |
| `assets/videos/2.mp4` | Original |
| `assets/videos/output.mp4` | Demo 1, A, and B |
| `assets/videos/kinect2.mp4` | Demo C and Claude |
| `assets/videos/3.mp4` | Preserved source; currently not linked by a page |
| `assets/videos/kinect3.mp4` | Preserved source; currently not linked by a page |

## Publishing

Publish the repository root as a static site. Hosts such as GitHub Pages will automatically use the root `index.html` as the home page. Keep the directory structure intact so all relative asset paths continue to work.

The home page has no build step and bundles its runtime dependencies locally. Third-party license files are in `assets/vendor/`.
