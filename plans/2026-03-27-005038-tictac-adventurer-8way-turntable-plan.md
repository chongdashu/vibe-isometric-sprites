# Tictac Adventurer 8-Way Turntable Plan

Linked prompts file: `prompts/2026-03-27-005038-tictac-adventurer-8way-turntable-prompts.md`
Linked learnings file: `learnings/2026-03-27-005038-tictac-adventurer-8way-turntable-learnings.md`

## Goal

Create a reusable 8-direction character turnaround workflow for the `tictac` adventurer: derive a full-body game asset from the portrait, generate an isometric tactics-style anchor, build curated cardinal and diagonal direction sheets with Nano Banana 2, assemble turntable media, and normalize the final frame set so direction changes do not cause visible scale drift.

## Approach

1. Generate the first full-body character asset from the portrait using local `gpt-image-1.5` edit tooling.
2. Generate an isometric tactics-style anchor using fal-hosted GPT Image 1.5 edit.
3. Evaluate small-frame readability and run a Nano Banana 2 simplification pass oriented toward `64x96` and `48x72` readability.
4. Generate a cardinal sheet from the isometric anchor using Nano Banana 2 with strict `N/E/S/W` ordering and chroma green output.
5. Curate the raw cardinal sheet into extracted per-direction frames and a clean 2x2 deliverable.
6. Generate a diagonal sheet from the curated cardinal chroma sheet plus the isometric anchor using Nano Banana 2.
7. Curate the raw diagonal sheet into extracted per-direction frames and a clean 2x2 `NW/NE/SW/SE` deliverable.
8. Assemble the 8-frame rotation order `N -> NE -> E -> SE -> S -> SW -> W -> NW` into turntable frames, contact sheet, transparent GIF, alpha MOV, and MP4 preview.
9. Measure visible alpha bounds for all eight frames and normalize them to a shared visible height and shared bottom anchor.
10. Rebuild derivative artifacts from the normalized frame set and refresh `public/assets/index.json`.

## Verification

- Visually confirm that the curated cardinal and diagonal sheets preserve character identity across views.
- Check that the 8-frame contact sheet reads in consistent rotational order and that diagonals no longer appear shorter after normalization.
- Confirm the transparent exports (`.gif`, `.mov`) preserve alpha and the MP4 preview renders correctly on a solid background.
- Verify the new assets are present in `public/assets/index.json`.
- Keep the raw fal experiment request/response artifacts under `experiments/fal-image/...` for reproducibility.
