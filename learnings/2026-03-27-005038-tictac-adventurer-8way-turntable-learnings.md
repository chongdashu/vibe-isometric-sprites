# Tictac Adventurer 8-Way Turntable Learnings

Related records:
- `plans/2026-03-27-005038-tictac-adventurer-8way-turntable-plan.md`
- `prompts/2026-03-27-005038-tictac-adventurer-8way-turntable-prompts.md`

## Context

This session established a full experimental pipeline for a new `tictac` adventurer character:

- portrait to full-body asset
- full-body asset to isometric tactics-style anchor
- anchor to cardinal and diagonal direction sheets
- direction sheets to 8-frame turntable media
- turntable frames to normalized, game-ready directional outputs

The work mixed model generation, deterministic curation, and light pipeline scripting.

## Initial Hypothesis

The working assumption was that Nano Banana 2 could generate direction sheets with enough consistency to bootstrap an 8-way turnaround, but would likely still need deterministic cleanup. A second assumption was that any remaining directional mismatch would be more about framing and scaling than about identity.

## What Happened

- The local `gpt-image-1.5` edit wrapper produced a strong full-body adventurer from the painted portrait and gave the workflow a stable identity source.
- fal GPT Image 1.5 produced the best isometric anchor. It preserved the costume and character better than Banana-family models and was the strongest base image for downstream direction work.
- Banana-family generation remained most useful as a chroma-sheet generator rather than a transparency-first renderer.
- In this environment, fal queue polling for several edit endpoints returned `405 Method Not Allowed`, even when submission succeeded. Direct synchronous POSTs to `https://fal.run/<endpoint>` worked reliably and still allowed request/response capture.
- Nano Banana 2 repeatedly ignored "exactly four poses" and instead returned 8-pose sheets for both the cardinal and diagonal requests.
- Those raw sheets were still valuable. Curating the cleanest subset into extracted direction frames produced usable `N/E/S/W` and `NW/NE/SW/SE` sets.
- The 8-frame turntable assembled from the curated outputs exposed a measurable normalization issue:
  - cardinals had visible heights around `495` to `502`
  - diagonals were shorter at roughly `472` to `479`
  - some diagonals also floated higher on the canvas
- Normalizing every frame to a shared visible height of `495` and a shared bottom anchor of `503` fixed the turntable "breathing" without requiring another generation pass.
- A small reusable script was enough for that normalization step and fit the repo's deterministic asset-pipeline style.

## Result Quality

- Best identity source: the portrait-derived full-body GPT Image result.
- Best isometric anchor: fal GPT Image 1.5 edit output.
- Best direction-sheet generator in this session: Nano Banana 2, but only when followed by curation.
- Best final turnaround deliverables: the normalized 8-frame turntable set in `public/assets/tictac/characters/adventurer-male/turntable-banana2-v1-normalized/`.
- The curated sheets are concept-usable and coherent enough for preview work, but they are still model outputs rather than perfectly orthographic, hand-authored runtime sprites.

## Decisions

- Use direct `fal.run` POSTs instead of the queue polling wrapper when the environment shows `405` for edit polling.
- Treat Banana-family outputs as chroma-key assets and split/curate them deterministically instead of trusting raw sheet layout.
- Preserve raw model outputs alongside curated deliverables so later re-curation stays possible.
- Normalize multi-direction frame sets with one shared target height and one shared bottom anchor before building turntables or runtime sheets.
- Keep transparency-preserving exports as GIF and alpha MOV; treat MP4 as a convenience preview format only.
