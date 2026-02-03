---
name: strudel
description: "Generate, edit, and troubleshoot Strudel (https://strudel.cc) live-coding music patterns. Use for: writing playable Strudel code (mini-notation + JS chaining), building layered grooves with sound()/note()/n()/scale(), structuring pieces with stack()/arrange(), and applying common FX (gain/lpf/room/delay/shape)."
---

# Strudel

## Quick Start

- Open `references/NAVIGATION.md` and follow the routing table.
- Prefer small, playable steps; keep edits reversible.

## Operating Principles (Non-Negotiable)

- Always ship a **minimal playable** pattern first; then add complexity (melody, syncopation, FX, structure).
- Do not hallucinate API/sounds. If unsure, verify in the Strudel editor (autocomplete/error output) or consult the vendored reference files.
- Prefer small, reversible edits (one layer/parameter at a time).
- Strudel syntax is strict (code must parse and functions must exist). Musical design is not: use the method/guides/examples as high-quality inspiration, not as constraints you must follow.

## Standard Workflow (0 → Playable)

1. Choose constraints: scale (e.g. `"E:minor"`), tempo (`setcpm(...)` or `.slow(...)`), vibe.
2. Pick roles: drums + bass + harmony + lead.
3. Build a skeleton with `stack(...)`.
4. Add harmony constraint via `.scale("<root>:<mode>")`.
5. Add groove using mini-notation + a small set of syncopation transforms.
6. Add FX slowly: `.gain()` / `.lpf()` / `.room()` / `.delay()` / `.shape()`.
7. Add structure using `arrange([count, section], ...)`.

## Where the Truth Lives (Inside This Skill)

- Entry point: `references/NAVIGATION.md`
- Full docs: `references/docs/`
- Function reference (INDEX + A-C/D-F/...): `references/ref/`
- Sound lists: `references/sounds/`
- Expert compositions: `references/compositions/`
- Methods: `references/methods/`
- Curated guides/snippets: `references/guides/`

## Function Lookup Rule (Anti-Hallucination)

When you are not 100% sure a function exists or how it behaves:

1. Search `references/ref/strudel_reference_INDEX.txt` for the function name.
2. Open the referenced alphabetical file and copy an example from there.
3. Only then incorporate it into the user-facing code.

## Default Output Template (Copy/Paste)

```js
const g_scale = "E:minor"

const intro = stack(
  n("0 2 1 3").scale(g_scale).sound("gm_pad_sweep").gain(0.6),
  n("-7 -5 -6 -4").scale(g_scale).sound("gm_pad_warm").gain(0.6),
  n("-2 ~ ~ 4 ~ ~ 3 ~ ~ 5 ~ ~").scale(g_scale).sound("gm_synth_bass_1").gain(0.6),
  n("7 ~ ~ 7 ~ ~ 5 ~ ~ 3 ~ ~").scale(g_scale).sound("supersaw").gain(0.8)
)

const main = stack(
  intro,
  s("[bd bd] <hh oh hh> [hh hh?] <bd sd> hh?").gain(1)
)

arrange(
  [2, intro],
  [8, main]
).slow(1.6)
```
