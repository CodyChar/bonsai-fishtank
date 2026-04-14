# Bonsai Fishtank

Head-coupled perspective ("fishtank VR") — as you move your head in front of
the monitor, the screen acts as a window onto a 3D scene of a bonsai tree.
Runs entirely in the browser with the front-facing webcam.

## How it works

1. MediaPipe Face Landmarker tracks your face in real time from the webcam
   and outputs a 4×4 facial transformation matrix in metric space.
2. The translation of that matrix is mapped into world coordinates as the
   position of your dominant eye.
3. Each frame the three.js camera is rebuilt with an asymmetric (off-axis)
   projection matrix using the Kooima (2009) construction, treating the
   screen rectangle as the near plane.

The tree doesn't rotate, you do — the projection math tells the graphics
card to shear the perspective so that the same 3D scene resolves into a
different 2D image depending on where your eye is.

## Usage

1. Open the page and click **Enable Camera**.
2. Grant camera access.
3. Adjust **Screen W (cm)** to match the real width of your monitor so the
   parallax is calibrated to physical reality.
4. If the tree drifts off-center, click **Recenter**.
5. Move your head; the tree should look like a real object sitting inside
   the monitor.

## Replacing the bonsai

Drop a `bonsai.glb` at the repo root and reload — it'll replace the
primitive-cylinder placeholder. Asset is auto-normalized to a 0.3 m bounding
box and placed on the pedestal.

Meshy (text-to-3D) exports GLB directly. Any GLB works.

## Stack

- [three.js](https://threejs.org/) for rendering
- [@mediapipe/tasks-vision](https://ai.google.dev/edge/mediapipe/solutions/vision/face_landmarker) for face tracking
- No build step, no server — static HTML, served by GitHub Pages

## Limits

- MediaPipe's metric scale is approximate; webcam FOV isn't calibrated, so
  lateral parallax magnitude depends on the hardware. The **Scale** slider
  lets you tune it by eye.
- Only one viewer. Multi-face tracking isn't wired up.
- Monocular tracking — there's no stereo rendering (you'd need a stereo
  display or a VR headset, which defeats the premise).
