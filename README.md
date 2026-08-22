# Scroll Video Website

## Turn any video into a smooth, scroll-controlled website with one Agent Skill.

[![Scroll Video Website demo](assets/demo.gif)](https://github.com/musoyangrigor/scroll-video-website-skill/blob/main/assets/demo.gif)

## Install

```bash
npx skills add musoyangrigor/scroll-video-website-skill --skill scroll-video-website
```

[Live Demo](#live-demo) · [Install](#install) · [Usage](#usage) · [Features](#features)

## Live Demo

[View the full demo GIF](https://github.com/musoyangrigor/scroll-video-website-skill/blob/main/assets/demo.gif)

## Example Website

The demo shows a simple product website made from a single video. The video fills the screen, and scrolling forward or backward smoothly moves through its frames.

You can create the same kind of website with:

```text
$scroll-video-website ./media/product-demo.mp4
```

The skill converts the video into optimized frames and builds the responsive scroll-controlled page for you.

## Usage

Give your agent the skill name followed by a video path:

```text
$scroll-video-website ./media/product-film.mp4
```

Optionally add visual direction after the path:

```text
$scroll-video-website ./media/product-film.mp4 use a dark editorial style
```

Optimize an existing generated frame sequence interactively:

```text
$scroll-video-website optimize
```

The command takes no arguments. The agent discovers generated sequences in the current project, measures the real assets, trial-encodes representative frames, and shows separate estimates for format, frame count, quality, and other supported settings. It then shows their combined estimated impact and asks for confirmation before rewriting anything.

## Features

- Converts video into an optimized WebP frame sequence.
- Scrubs smoothly in both directions with progressive frame loading.
- Renders a responsive, full-viewport canvas with adjacent-frame blending.
- Preserves the existing project stack and supports reduced-motion preferences.
- Builds a minimal canvas-only experience by default, or follows added design direction.
- Interactively optimizes existing sequences with file-specific size estimates before processing.

MIT licensed. See [LICENSE](LICENSE).
