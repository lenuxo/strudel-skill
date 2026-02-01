# Workflow & Verification (Standalone)

## First decision: “Compose” vs “Explain/Fix”

- Compose: output a minimal playable sketch first, then offer 1–2 upgrade paths (syncopation, FX, structure).
- Explain/Fix: isolate the exact syntax/API issue, then propose the smallest change that makes it playable.

## How to Verify When You’re Unsure

Because this skill is meant to be portable, treat the **Strudel editor** as the source of truth:

- Use autocomplete / docs panel / error output to confirm function names and parameter shapes.
- If a chain method doesn’t exist, back off to primitives: `sound`/`s`/`note`/`n` + `scale` + `stack` + `arrange`.
- Keep a minimal fallback pattern (e.g. a simple drum loop) to validate the session is healthy.

## Pre-Output Checklist

- The code is short, playable, and uses known primitives.
- Layers are readable (one role per layer: drums, bass, harmony, lead).
- Tempo is controlled (`setcpm` or `.slow(...)`).
- FX are small and incremental (avoid stacking multiple same FX expecting it to “chain”).
- If you added complexity, you can remove it without breaking everything (reversible edits).
