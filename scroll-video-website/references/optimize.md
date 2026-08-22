# Optimize a frame sequence

Use this workflow only for the exact invocation `$scroll-video-website optimize`. The command takes no path or other arguments. Optimization is interactive: inspect and estimate first, let the user choose, and make no asset changes until they confirm the combined proposal.

## Inspect the sequence

Search the current project for numbered frame sequences. Ignore dependency, build-cache, and VCS directories. If multiple plausible sequences exist, show them and ask the user which one to use.

If no sequence exists, or the agent cannot confidently recognize which assets constitute the generated frame sequence, stop and tell the user:

```text
Frames folder was not found.
```

Use the same response when `optimize` is invoked before this skill has generated frames for the project. Do not ask for a path or attempt to optimize unrelated images.

Measure the actual files rather than relying on source-video metadata:

- format, frame count, dimensions, alpha usage, and total byte size;
- numbering pattern and the application references that determine URLs and frame count;
- available local encoders and the settings they support.

Preserve animation order and aspect ratio. Do not assume the current encoder quality can be recovered from an encoded file.

## Produce evidence-based estimates

Use temporary files outside the sequence directory. Select evenly spaced frames across the entire animation, including the first and last; use enough samples to represent changes in visual complexity. Twenty-four samples is a reasonable default, but increase or decrease it for unusually varied or very short sequences.

Trial-encode the sampled source frames with the actual local encoder and each candidate setting. Estimate output bytes from the measured encoded-to-source byte ratios. Weight estimates by the original byte sizes, or partition the timeline into intervals represented by each sample and sum the interval estimates. Never substitute a fixed format or quality percentage.

Estimate each choice independently against the current sequence, changing only that setting:

- **Output format:** trial-encode to supported browser formats such as AVIF or WebP. Preserve alpha when present.
- **Frame count:** choose frames evenly over the full timeline and sum the actual sizes of the retained frames when no re-encoding is involved. If interpolation, scaling, or re-encoding is required, trial the real operation on representative frames.
- **Quality/compression:** trial several useful encoder quality levels in the current or selected format. Label encoder-specific scales clearly; quality numbers are not portable between codecs.
- **Other available settings:** offer only settings supported by the detected tools and useful for these assets, such as maximum dimensions, lossless/lossy mode, chroma subsampling, encoder effort, or metadata removal. Warn when a choice can affect sharpness, color, alpha, encoding time, or browser compatibility.

For the combined estimate, run the selected settings together on the representative frames and account for the selected frame-retention pattern. Do not multiply or add the independent percentages: codec, quality, dimensions, and frame selection interact.

Use byte totals internally. Display human-readable values consistently and calculate reduction as:

```text
reduction = (originalBytes - estimatedBytes) / originalBytes * 100
```

State that estimates are projections and mention the sample count. For a very small sequence, trial-encode every frame; that estimate is then an exact encoded-size preview, excluding incidental filesystem or manifest overhead.

## Ask the user

Present the current measurements and useful choices with a separate impact estimate for every candidate. Ask for their preferred:

1. output format;
2. frame count;
3. quality/compression level;
4. any other applicable settings discovered during inspection.

Use concrete prompts in this shape, adapting values to the actual sequence:

```text
Convert WebP to AVIF?
Current size: 2.8 MB → Estimated size: 1.3 MB (~54% smaller)

Reduce frames?
Current frames: 240 → Proposed frames: 150
Current size: 2.8 MB → Estimated size: 1.2 MB (~57% smaller)
```

After the user selects settings, recompute any estimates affected by their choices and show one final confirmation summary:

```text
All selected optimizations combined
2.8 MB → 0.7 MB (~75% smaller)
```

Include the final format, frame count, quality, dimensions, and other selected settings. Ask for confirmation before processing. A request to inspect or estimate does not authorize rewriting assets.

## Apply safely

Generate the complete optimized sequence in a temporary sibling directory. Validate frame count, dimensions, decodability, ordering, alpha behavior, first and last frames, and total size. Compare representative frames visually when possible.

Only after validation, replace the sequence using a recoverable swap or retain a backup unless the user explicitly declines one. Update application paths, extensions, manifests, preload lists, and frame-count constants together. Do not delete the source video or unrelated assets.

Report the measured final size and reduction, which may differ from the estimate, along with the settings applied and checks run.
