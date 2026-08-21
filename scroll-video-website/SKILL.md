---
name: scroll-video-website
description: Build or redesign an existing website around a user-provided video, using smooth scroll-controlled video scrubbing as the full-viewport primary visual. Use for cinematic product pages, landing pages, and storytelling sites driven by a video timeline; do not use for ordinary autoplay video backgrounds or non-video scroll effects.
---

# Scroll Video Website

Create a premium, minimal website in which scrolling controls a full-viewport video. Treat the supplied video as the primary storytelling surface, not as decoration behind a conventional interface.

## Establish the project and source video

Before editing:

1. Inspect the project structure, package scripts, framework, styling system, routing, and relevant local instructions.
2. Locate the video the user supplied. If no video or path was provided and none can be confidently identified, ask for it before implementing.
3. Reuse the existing stack, asset conventions, and working project. Do not scaffold a replacement project when one already works.
4. Preserve unrelated behavior and make the smallest coherent set of changes needed for the redesign.
5. Inspect the video when practical—format, dimensions, duration, file size, and visual sequence—to inform composition and section timing. Do not recreate or replace it.

Place or reference the video according to the project's existing asset strategy. Avoid duplicate large media files unless copying is necessary for the build system.

## Design around the footage

Default to a clean, premium landing page with about four sections when no direction or copy is supplied. Keep the overall experience to roughly four or five sections, with minimal copy, large typography, restrained transitions, and deliberate whitespace. Align each section's message and placement with an important portion of the footage.

Keep the video visually dominant. Use overlays or subtle localized contrast treatments only when text readability requires them. Avoid card grids, dashboard patterns, excessive navigation or calls to action, random illustrations, ornamental 3D, and gratuitous gradients. Do not add an animation dependency when native browser APIs are sufficient.

Use responsive composition deliberately. Ensure text remains legible against cropped footage, account for mobile viewport behavior and safe areas, and reduce heavy visual effects on constrained devices.

## Implement scroll-driven scrubbing

The video must:

- fill the viewport and remain visually fixed or sticky while the document scrolls;
- be `muted` and `playsInline`, use `object-fit: cover`, and preload enough data for scrubbing;
- remain paused—never call `video.play()`;
- be controlled by assigning `video.currentTime` manually;
- map clamped total page or experience progress from `0..1` onto `0..video.duration`, so reverse scrolling reverses the video.

Wait for `loadedmetadata` before using duration or seeking. Derive progress from the actual scroll range, such as `scrollY / (documentElement.scrollHeight - innerHeight)`, guarding against a zero range and non-finite duration.

Do not assign `currentTime` directly in every scroll event. Scroll and resize handlers should only update inexpensive measurements or a target. Run one `requestAnimationFrame` loop that advances an internal playhead with frame-rate-independent exponential smoothing:

```js
const alpha = 1 - Math.exp(-dt * 8)
current += (target - current) * alpha
video.currentTime = current
```

Compute `dt` in seconds and clamp unusually large frame gaps so tab switches do not cause jarring jumps. Clamp the target and playhead to the seekable timeline. Stop writing once sufficiently close to the target when useful, but resume the loop promptly after scrolling. Prefer refs or mutable local values over framework state so scrolling does not cause component rerenders.

Use passive scroll listeners where appropriate. Recalculate scroll range on resize and when layout height can change. Clean up all listeners, media-query listeners, observers, and animation frames when the owning component unmounts.

Do not let normal playback, repeated initialization, or competing effects write to the playhead. Avoid flashing before metadata loads; render a suitable poster/background or keep the video surface intentionally styled until the first frame is available.

## Accessibility and fallbacks

For `prefers-reduced-motion: reduce`, disable scroll scrubbing and hold a suitable static frame, normally the first frame. Keep the page content usable without motion. Do not introduce scroll hijacking.

If the source codec, keyframe spacing, bitrate, or file size makes seeking visibly poor, explain that the video needs web optimization rather than masking the limitation with more animation code. Recommend a browser-compatible encoding with frequent enough keyframes and an appropriate bitrate, while preserving the user's original source.

## Verify the result

Run the project's relevant checks and inspect the experience in a browser when available. Confirm:

- the first and last scroll positions correspond to the beginning and end of the video;
- forward and reverse scrolling are smooth, without abrupt seeks, restarts, flashing, or playback fighting the scrubber;
- metadata loading, resize, route/component cleanup, and reduced-motion behavior work;
- desktop and mobile layouts preserve video prominence and readable, sparse content;
- unrelated project functionality still works.

Finish by summarizing the implemented experience, checks run, and any source-video encoding limitation that remains.
