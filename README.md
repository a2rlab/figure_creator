# Figure Bench

Turn robot experiment videos into publication-ready figures: snapshot sequences with time labels, trajectories, ghost (stroboscopic) composites, method comparisons, and supplementary videos.

**Open the tool:** https://YOUR-USERNAME.github.io/figure-bench/

## Your videos stay on your computer

Figure Bench is a single web page. Your browser downloads the page once. After that, all processing (decoding, tracking, compositing, exporting) runs locally on your machine. Videos are never uploaded, and there is no server to upload them to.

After the first visit, the page also works with no internet connection. In Chrome or Edge you can install it as an app: click the install icon in the address bar.

You can check this yourself. Open the browser's developer tools (F12), go to the **Network** tab, and process a video. No requests go out.

## Requirements

- A current Chrome, Edge, Firefox, or Safari on Windows, macOS, or Linux.
- Videos in **H.264 MP4**, which is what most cameras and phones produce.
- A fixed camera (tripod). A moving camera breaks the ghost composite and automatic tracking.

If a video will not open, it is probably HEVC/H.265. Re-encode it:

```
ffmpeg -i input.mp4 -c:v libx264 -crf 18 -pix_fmt yuv420p output.mp4
```

## Quick start: snapshot figure

1. **Videos:** load your MP4 and set the **frame rate** to match the recording.
2. **Time range:** scrub to where the motion starts and press **Use current** for Start. Do the same for End.
3. **Snapshots:** choose how many panels and the layout, for example 6 panels in two rows.
4. **Crop:** press **Draw crop box** and drag around the area that matters. The same crop applies to every panel.
5. Press **Process**, then **Download PNG** or **Download PDF**.

The figure is exported at the real column width of your target journal (step 9), so fonts appear at their true size in LaTeX:

```latex
\includegraphics[width=\linewidth]{myvideo_snapshots_300dpi.png}
```

## Trajectory

1. In step 6, press **Mark robot position** and click the robot's centre. The video jumps ahead (0.5 s by default). Keep clicking until the end, and press Esc to stop.
2. Alternatively, mark the robot once and press **Track from this frame**:
   - **Follow appearance** works for most robots.
   - **Follow motion** is better when the robot turns and changes its look. It uses the ghost background.
3. If tracking loses the robot, it stops and tells you the time. Mark the robot there by hand, then track again from that frame.
4. To fix a point, click its yellow dot on the timeline to jump to it, then click the correct position.
5. **Export CSV** saves the points (time, frame, x, y in pixels) for your own analysis.

Colour the path **by time** to get a viridis gradient and a time colour bar under the figure.

## Ghost composite

Set **Output** (step 3) to **Ghost composite** or **Grid + ghost panel**, then press **Process**. The robot from every snapshot is layered on one clean background, faint early and solid at the end.

- Turn on **Highlight detected areas** while tuning. Red should cover the robot and nothing else.
- Raise **Threshold** if shadows, reflections, or people appear. Lower it if parts of the robot are missing.
- The background is the median of many frames. If the robot stays still for most of the range, it becomes part of the background. Widen the time range or increase **Background** frames.
- Crop away moving people and screens. They show up as ghosts too.

## Comparing methods

1. Load several videos (step 1). Give each a name, for example *Baseline* and *Ours*. The names appear in the figure.
2. Press **Edit** on each video in turn and set its time range and trajectory. Starting each range at the moment the robot starts moving makes the rows line up.
3. Set **Output** to:
   - **Comparison, one row per video**, which places panels at the same snapshot times in each row, or
   - **All paths on one background**, which shows every trajectory in its own colour with a legend. Use this only when all videos come from the same camera position.

## Supplementary video

Step 10 records the time range with the crop, time label, speed-up label, and growing trajectory. Choose **All, side by side** for a synchronized comparison. Recording runs in real time at the chosen speed (a 60 s range at 4× takes 15 s), so keep the tab visible. The output is MP4 when the browser supports it, otherwise WebM.

## Consistent figures across a paper

Set fonts, sizes, colours, and output width once, then press **Save style**. Load that file for every figure in the paper so they all match. Keep it next to your LaTeX source.

Tips for a clean scientific look:

- Use **Times** to match a LaTeX body, at 8 pt for figures.
- Export at **300 dpi** for photos; use 600 dpi only if the publisher asks.
- Use the viridis time colouring rather than custom rainbows. It is perceptually uniform and readable for colour-blind readers and in greyscale print.

## Keyboard shortcuts

| Key | Action |
| --- | --- |
| ← / → | Step one frame |
| Shift + ← / → | Step one second |
| Space | Play / pause |
| Ctrl/Cmd + Z | Undo trajectory point |
| Esc | Leave the current drawing tool |

## Known limitations

- Trajectories are in image coordinates, so with a perspective (low-angle) camera, distances far from the camera look shorter.
- Frame-exact seeking depends on the browser's video decoder. Chrome and Edge are the most precise.
- PDF export contains a high-resolution image, not vector text.
- Video recording quality depends on the computer keeping up in real time. Use a lower speed or 1280 px width on slower machines.

To update:

1. Replace `index.html`.
2. Change `VERSION` at the top of `sw.js`, for example `fb-v3` → `fb-v4`.

Students get the new version the next time they open the page while online.
