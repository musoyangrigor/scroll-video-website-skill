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

## Usage

Give your agent the skill name followed by a video path:

```text
$scroll-video-website ./media/product-film.mp4
```

Optionally add visual direction after the path:

```text
$scroll-video-website ./media/product-film.mp4 use a dark editorial style
```

## Features

- Converts video into an optimized WebP frame sequence.
- Scrubs smoothly in both directions with progressive frame loading.
- Renders a responsive, full-viewport canvas with adjacent-frame blending.
- Preserves the existing project stack and supports reduced-motion preferences.
- Builds a minimal canvas-only experience by default, or follows added design direction.

MIT licensed. See [LICENSE](LICENSE).
