# Anchor Framework (Standalone Composition Method)

Goal: produce a **playable, extensible** sketch fast, with clear harmonic anchors and room for live iteration.

## Core Shape (4 Layers)

- Layer 1: 4 steps (harmony / mid register)
- Layer 2: 4 steps (bass support / low register)
- Layer 3: 12 steps (melody; anchors on steps 1/4/7/10)
- Layer 4: 12 steps (counter-melody; anchors on steps 1/4/7/10)

Key idea: patterns with different step counts still align to the same cycle; a 12-step line “moves 3x faster” than a 4-step line.

## Iteration Plan (Strict Order)

1. Step 0 (Anchors only): write all 4 layers with a single neutral sound (often `piano`).
   - For 12-step layers, fill non-anchor positions with `~`.
2. Step 1 (Timbre pass): swap sounds for each layer (pad/bass/lead/drums).
3. Step 2 (Fill melody): turn Layer 3 into a full 12-step line while keeping anchor harmony.
4. Step 3 (Add syncopation): apply a few standard transforms to Layer 3.
5. Step 4 (Counter line): add sparse notes to Layer 4; optionally echo the syncopation style.
6. Step 5 (Light groove on harmony): add minimal syncopation to Layers 1–2.
7. Step 6 (FX pass): add small amounts of reverb/filter/distortion.
8. Step 7 (Drums + structure): add drums, then `arrange(...)` sections.

## Syncopation Transforms (Use Sparingly)

Replace a single event `note` with one of these:

```txt
note -> [~ note]
note -> [note ~]
note -> [note@2 ~]
note -> [~ note@2]
```

Rule of thumb: apply to 2–4 positions per cycle first. Too many transforms can destroy the groove.

## Minimal Anchor Template

```js
const g_scale = "E:minor"

const step0 = stack(
  n("0 2 1 3").scale(g_scale).sound("piano"),
  n("-7 -5 -6 -4").scale(g_scale).sound("piano"),
  n("-2 ~ ~ 4 ~ ~ 3 ~ ~ 5 ~ ~").scale(g_scale).sound("piano"),
  n("7 ~ ~ 7 ~ ~ 5 ~ ~ 3 ~ ~").scale(g_scale).sound("piano")
)
```

## Full Worked Examples

For complete, step-by-step versions (anchors → melody → syncopation → FX → drums → arrange), read:
- `references/walkthroughs.md`
