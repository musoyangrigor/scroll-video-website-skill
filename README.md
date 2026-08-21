# Scroll Video Website

Build minimal, cinematic websites where scrolling smoothly scrubs a full-viewport canvas animation extracted from a supplied video.

This portable Agent Skill guides coding agents to inspect an existing website, preserve its stack and unrelated behavior, convert the user's video into an optimized WebP frame sequence, and render it on a fixed canvas with bidirectional, smoothed scroll control. With no additional design direction, it produces a clean canvas-only experience with the same shape as the TRIO animation reference: white stage, approximately `400vh` of scroll space, cover-cropped imagery, progressive frame loading, and no invented interface or marketing copy.

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
$scroll-video-website ./media/product-film.mp4
```

The path immediately follows the skill name and is resolved relative to the current project. Additional prompt text can supply visual direction:

```text
$scroll-video-website ./media/product-film.mp4 use a dark editorial style with minimal copy
```

A unique bare filename match may live in a subdirectory, and an absolute path also works. Quote paths containing spaces. If no video is supplied, the file is missing, or the filename is ambiguous, the skill asks for a precise path.

## What it enforces

- A fixed, full-viewport canvas backed by a numbered WebP frame sequence.
- Video-to-frame extraction at a practical cadence, size, and quality.
- Progressive loading of the first frame, distributed timeline frames, then the remainder.
- Bidirectional page-progress mapping, adjacent-frame blending, and frame-rate-independent smoothing.
- A minimal canvas-only default instead of invented navigation, cards, or copy.
- Existing-framework reuse, cover-cropped responsive rendering, reduced-motion fallback, and complete cleanup.

The skill follows the portable `SKILL.md` Agent Skills format.
