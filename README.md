# Scroll Video Website

> Build or redesign a website around a user-provided video as a smooth, scroll-controlled full-viewport canvas frame sequence. Use for cinematic product reveals and storytelling pages where scrolling should scrub an animation; do not use for ordinary autoplay video backgrounds or unrelated scroll effects.

Turn a video into a smooth, scroll-controlled canvas website.

Scroll Video Website is a portable Agent Skill that converts a supplied video into an optimized WebP frame sequence and builds a responsive, full-viewport animation controlled by scrolling. It preserves the existing project stack while handling frame extraction, progressive loading, bidirectional scrubbing, interpolation, reduced-motion behavior, and cleanup.

## Demo

![Scroll Video Website demo](assets/demo.gif)

## Install

```bash
npx skills add musoyangrigor/scroll-video-website-skill --skill scroll-video-website
```

The Skills CLI configures the skill for the selected supported AI agent, including Codex. Start a new agent session after installation.

## Usage

Place the video path immediately after the skill name:

```text
$scroll-video-website ./media/product-film.mp4
```

Add optional design direction after the path:

```text
$scroll-video-website ./media/product-film.mp4 use a dark editorial style with condensed typography
```

| Invocation | Result |
| --- | --- |
| `$scroll-video-website <video-path>` | Build the default minimal scroll-animation website. |
| `$scroll-video-website <video-path> <instructions>` | Build the same animation architecture while following the added style, layout, or copy direction. |

Paths are resolved relative to the current project unless they are absolute. Quote paths containing spaces. If a bare filename has exactly one match in the project, the skill can locate it automatically.

## What it builds

- A fixed, full-viewport canvas backed by numbered WebP frames.
- Smooth forward and reverse scrubbing tied to total page progress.
- Progressive frame loading with nearby-frame fallbacks.
- Adjacent-frame blending and frame-rate-independent smoothing.
- Responsive cover rendering with device-pixel-ratio support.
- A static accessible fallback for reduced-motion preferences.
- A minimal canvas-only page by default, without invented navigation or marketing copy.

## About

Scroll Video Website follows the portable `SKILL.md` Agent Skills format.
