# Beat-Sync VFX — Neural Body Outline Edition

This version replaces the HOG-first body outline with **person instance segmentation**. By default it uses `yolo11n-seg.pt` through Ultralytics. If Ultralytics or the weights are unavailable, it automatically falls back to HOG + GrabCut.

## Why the outline is better

- Pixel-level person segmentation rather than bounding-box edge tracing.
- Multiple people are supported by merging person-instance masks.
- Optical flow propagates the previous mask between neural detections.
- EMA temporal smoothing reduces outline flicker.
- Morphological cleanup removes speckles and fills small holes.
- `approxPolyDP` lightly smooths contours before a multi-scale neon glow is rendered.

## Install

```bash
pip install -r requirements.txt
```

Install FFmpeg separately if it is not already available.

## Configure

Edit these variables near the top of `beat_sync_vfx_neural_outline.py`:

```python
VIDEO_PATH = "/path/to/video.mp4"
AUDIO_PATH = "/path/to/music.mp3"
OUTPUT_PATH = "output_video_neural_outline.mp4"
SEGMENTATION_MODEL = "yolo11n-seg.pt"
```

For better silhouettes and a capable GPU, use a larger compatible segmentation checkpoint. `SEGMENT_EVERY = 1` gives the highest temporal accuracy; `2` or `3` is faster.

## Run

```bash
python beat_sync_vfx_neural_outline.py
```

## Notes

The first Ultralytics run may download the selected model if the checkpoint is not already present. In an offline environment, place the `.pt` file locally and set `SEGMENTATION_MODEL` to that path. If the model cannot load, the script remains functional via the fallback detector.
