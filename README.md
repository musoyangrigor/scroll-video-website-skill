# Scroll Video Website

Build premium, minimal websites where scrolling smoothly controls a full-viewport video timeline.

This portable Agent Skill guides coding agents to inspect an existing website, preserve its stack and unrelated behavior, locate the user-provided video, and redesign the experience around cinematic bidirectional scroll scrubbing. It includes responsive, reduced-motion, cleanup, and video-encoding guidance.

## Install

```bash
npx skills add musoyangrigor/scroll-video-website --skill scroll-video-website
```

Install globally for Codex without prompts:

```bash
npx skills add musoyangrigor/scroll-video-website --skill scroll-video-website -g -a codex -y
```

Start a new agent session after installation, then invoke it with:

```text
$scroll-video-website Build this site around ./public/product-film.mp4
```

If no video is supplied, the skill asks for the file or its path before implementation.

## What it enforces

- A muted, inline, full-viewport video that remains fixed or sticky while scrolling.
- Manual paused-video seeking; normal playback is never started.
- Bidirectional mapping from page progress to the full video timeline.
- Frame-rate-independent playhead smoothing in `requestAnimationFrame`.
- Minimal, cinematic design with the video as the main storytelling element.
- Existing-framework reuse, responsive behavior, reduced-motion fallback, and complete cleanup.

The skill follows the portable `SKILL.md` Agent Skills format.
