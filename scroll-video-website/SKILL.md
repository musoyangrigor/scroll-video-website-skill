---
name: scroll-video-website
description: Turn a user-provided video into an Apple-style, scroll-driven website with smooth canvas video scrubbing, optimized WebP frame sequences, progressive loading, and responsive motion. Use for cinematic product reveals, landing pages, scrollytelling, image-sequence animation, and scroll-controlled video; also optimize generated frame assets.
---

# Scroll Video Website

## When to use

Use this skill when the user wants to:

- build an Apple-style scroll animation from a video;
- create a scroll-controlled or scroll-scrubbed video website;
- turn a product video into a cinematic landing page;
- build a scrollytelling experience driven by video;
- convert video into an optimized image sequence for scroll animation;
- create a Canvas-based product reveal controlled by scrolling;
- optimize an existing generated scroll-video frame sequence.

Do not use it for ordinary autoplay video backgrounds or unrelated scroll effects.

Create a minimal full-page experience like a product animation viewer: the page itself supplies scroll distance while a fixed canvas fills the viewport and scrubs a frame sequence derived from the user's video. The animation is the interface, not a background behind a conventional landing page.

## Invocation

This skill has two modes: build a scroll-video website from a video, or optimize an existing frame sequence. If the first argument is `optimize`, follow [references/optimize.md](references/optimize.md) and do not run the build workflow below.

Treat this concise form as a complete build request:

```text
$scroll-video-website <path/to/video.mp4>
```

The video path must be the first argument after the skill name. Everything after the path is optional user direction for styling, layout, copy, or behavior:

```text
$scroll-video-website ./media/product-reveal.mp4
$scroll-video-website ./media/product-reveal.mp4 use a black background and condensed typography
```

The first example means: derive the frame sequence and build the full default scroll-animation website described below. In the second, preserve the same video-driven animation architecture while following the added visual direction. Let the agent using this skill interpret and implement the rest of the prompt; this skill does not define a separate command grammar for style options.

Interpret the video path as relative to the current working directory unless it is absolute. If it contains spaces, the user should quote it. Check the exact path first. If a bare filename is not there, search the current project for an exact filename match while excluding dependency, build, cache, and VCS directories. Continue automatically when exactly one match exists. If there are no matches or multiple matches, ask the user for the precise path. Braces in `<path/to/video.mp4>` or `{video_path}` indicate a placeholder and are not part of the actual path. Do not require or interpret a `use` keyword.

Optimize an existing sequence with exactly:

```text
$scroll-video-website optimize
```

The optimize command takes no path or other arguments. Inspect the current project for generated frame sequences. Continue when exactly one sequence is identifiable. If several are identifiable, show them and ask which one to optimize. If no sequence can be found or confidently recognized, tell the user that the frames folder was not found and stop.

## Establish the project and source video

Before editing:

1. Inspect the project structure, scripts, framework, styling, routing, asset conventions, and local instructions.
2. Locate the user-supplied video using the invocation rules above. If no video or path was supplied and none can be confidently identified, ask for it before implementing.
3. Reuse the existing working stack. Do not scaffold a replacement project when one already works.
4. Inspect the video's dimensions, duration, frame rate, frame count, and file size with `ffprobe` when available.
5. Preserve unrelated behavior and make the smallest coherent set of changes needed.

Do not create substitute artwork or replace the supplied footage. Avoid duplicating the source video unless the build requires it.

## Convert the video to a frame sequence

The default rendering path is a numbered WebP frame sequence, not `video.currentTime`. Extract frames into the project's public/static asset area with zero-padded names such as `frames/frame_0001.webp`.

- Preserve the source cadence up to 30 fps. For a long clip, sample it to roughly 240–300 total frames so the payload remains practical.
- Preserve the source aspect ratio. Use a sensible WebP quality around 78–82 and inspect the result rather than assuming the setting is adequate.
- Record the actual extracted frame count in the implementation; do not leave a guessed constant.
- Prefer `ffmpeg` for extraction. A representative command is `ffmpeg -i input.mp4 -vf fps=30 -c:v libwebp -quality 80 frames/frame_%04d.webp`; adapt the input, fps, output path, and existing project tooling.
- Do not delete or overwrite unrelated assets. If the destination already contains a sequence, verify that it belongs to this animation before replacing it.

Use direct paused-video seeking only when frame extraction is unavailable or the resulting frame payload is clearly unsuitable. If falling back, explain the tradeoff and retain the same fixed-stage, bidirectional, smoothed scroll behavior.

## Match the reference experience

When the user gives no additional layout or copy direction, produce a canvas-only page:

- a white page and fixed, overflow-hidden stage covering `100vw` by `100vh`;
- enough invisible document height for a deliberate scrub, approximately `400vh` for an 8-second or roughly 240-frame source;
- no navbar, cards, headings, calls to action, gradients, or invented marketing copy;
- a canvas that fills the stage and draws each frame with `cover` geometry, centered on both axes;
- a short opacity reveal only after the first frame is ready, preventing a blank-frame flash;
- restrained image treatment only when it suits the footage, such as a white backdrop, a slight contrast adjustment, or `mix-blend-mode: multiply` for a white-background product render.

If the user asks for content, keep it sparse and place it over or between intentional moments in the animation without reducing the canvas's visual dominance. Do not introduce an animation dependency when native canvas and browser APIs are sufficient.

## Implement canvas scrubbing

Use a fixed or sticky stage containing an `aria-hidden` canvas. Size its backing buffer for device pixel ratio, capped around 2, while keeping drawing measurements in CSS pixels. Reset the canvas transform before applying a new DPR scale on resize so scaling does not accumulate.

Map clamped total-page progress to the complete sequence:

```js
const range = document.documentElement.scrollHeight - innerHeight
const progress = range > 0 ? Math.min(1, Math.max(0, scrollY / range)) : 0
targetFrame = progress * (frameCount - 1)
```

Scrolling forward must advance the animation and reverse scrolling must reverse it. Scroll and resize handlers should only update cheap measurements or the target. Run a single `requestAnimationFrame` loop and smooth a floating-point playhead with frame-rate-independent exponential interpolation:

```js
const dt = Math.min((now - lastTime) / 1000, 0.1)
const alpha = 1 - Math.exp(-dt * 9)
currentFrame += (targetFrame - currentFrame) * alpha
```

Draw the integer frame on each side of the playhead and crossfade the next frame by the fractional remainder. Clear or paint the canvas background first, reset `globalAlpha` after drawing, and redraw on resize.

## Load frames progressively

Do not block first paint on the entire sequence:

1. Load frame 1 first, draw it, and reveal the canvas.
2. Queue evenly distributed frames across the timeline, for example every eighth frame, so seeking has a nearby fallback early.
3. Queue every remaining frame.

Track requested frames separately from loaded frames so the priority and remainder passes never request the same image twice. While a requested frame is still loading, draw the nearest loaded frame. Handle failed image loads without stalling the rest of the sequence.

Use an asset URL strategy that works with the project's base path and deployment target. Clean up scroll, resize, media-query listeners, observers, and animation frames when the owning component unmounts.

## Responsive behavior and accessibility

Keep `cover` rendering on narrow screens and accept deliberate cropping; do not distort the imagery. Check important subject placement at desktop and mobile sizes. Account for mobile viewport changes and safe areas if visible content is added.

For `prefers-reduced-motion: reduce`, stop scrubbing and hold the first suitable frame. Keep any page content accessible and do not hijack scrolling. The decorative canvas should not create redundant screen-reader content.

## Verify the result

Run the project's checks and inspect the result in a browser when available. Confirm:

- the first and last scroll positions reach the first and last extracted frames;
- forward and reverse scrolling feel continuous, with no abrupt jumps, playback, flashing, or visible blank canvas;
- frame requests are not duplicated and missing or slow frames use a nearby loaded fallback;
- cover cropping, DPR resizing, and canvas scaling work at desktop and mobile sizes;
- reduced motion holds a static frame and component cleanup is complete;
- the generated sequence has the expected count, dimensions, ordering, and acceptable total size;
- unrelated project behavior still works.

Finish by summarizing the experience, the extracted frame count and payload, checks run, and any remaining source-media limitation.
