---
name: strudel
description: "Generate, edit, and troubleshoot Strudel (https://strudel.cc) live-coding music patterns. Use for: writing playable Strudel code (mini-notation + JS chaining), building layered grooves with sound()/note()/n()/scale(), structuring pieces with stack()/arrange(), and applying common FX (gain/lpf/room/delay/shape)."
---

# Strudel

## Operating Principles (Non-Negotiable)

- Always ship a **minimal playable** pattern first; then add complexity (melody, syncopation, FX, structure).
- Do not hallucinate API/sounds. If unsure, **verify in the Strudel editor** (autocomplete, docs panel, error output) before committing to a function name.
- Prefer **small, reversible edits** (one layer/parameter at a time) to keep live iteration stable.

## Standard Workflow (0 → Playable)

1. Set constraints: key/scale (e.g. `"E:minor"`), tempo (`setcpm(...)` or final `.slow(...)`), vibe.
2. Pick sound sources: drums + bass + harmony pad + lead.
3. Build a skeleton with `stack(...)`.
4. Add harmonic constraints via `.scale("<root>:<mode>")` (and optionally chord/voicing tools if you know them).
5. Add groove with mini-notation and a small set of syncopation transforms.
6. Add FX slowly: `.gain()` / `.lpf()` / `.room()` / `.delay()` / `.shape()`.
7. Add structure with `arrange([count, section], ...)`.

## Mini-Notation Quick Reference (Essentials)

Use these in `sound("...")`, `s("...")`, `note("...")`, `n("...")`:

- Sequence: space-separated tokens (e.g. `"bd hh sd hh"`)
- Rest: `~` or `-`
- Sub-sequence: `[ ... ]` (nested rhythm inside a step)
- Parallel layers: `,` (e.g. `"bd*4, hh*8"`)
- Alternation: `< ... >` (cycle options over cycles)
- Speed up: `*` (e.g. `"hh*8"`)
- Slow down: `/` (e.g. `"[c a f e]/2"`)
- Elongate duration: `@` (e.g. `"c@2"`)
- Repeat: `!` (e.g. `"c!3"`)
- Euclidean: `(k,n[,offset])` (e.g. `"bd(3,8)"`)

## Core Primitives (What You Reach For First)

- `sound("...")` / `s("...")`: trigger samples/drums via mini-notation.
- `note("...")`: play pitches by name or number.
- `n("...")`: use for sample indices or scale degrees (commonly paired with `.scale(...)`).
- `.scale("E:minor")`: map degrees to pitches.
- `stack(...)`: layer patterns.
- `arrange([count, section], ...)`: sequence sections.
- `setcpm(...)`: set tempo (cycles per minute).

## Anchor Framework (Recommended Composition Scaffold)

When the user wants a “full musical idea” quickly, use Anchor Framework:

- 4 layers in a single `stack(...)`:
  - Layer 1: 4 steps (harmony / mid)
  - Layer 2: 4 steps (bass support)
  - Layer 3: 12 steps (melody; anchors on steps 1/4/7/10)
  - Layer 4: 12 steps (counter-melody; anchors on steps 1/4/7/10)
- Start with an **anchor-only** version (use `~` for non-anchors), then fill notes and add groove.

Full method + transforms: read `references/anchor-framework.md`.

## Default Output Template (Copy/Paste)

Copy and edit `g_scale`, sounds, and patterns:

```js
const g_scale = "E:minor"

const intro = stack(
  // Instrument 1: 4-step harmony
  n("0 2 1 3").scale(g_scale).sound("gm_pad_sweep").gain(0.6),
  // Instrument 2: 4-step bass support
  n("-7 -5 -6 -4").scale(g_scale).sound("gm_pad_warm").gain(0.6),
  // Instrument 3: 12-step melody anchors (1/4/7/10)
  n("-2 ~ ~ 4 ~ ~ 3 ~ ~ 5 ~ ~").scale(g_scale).sound("gm_synth_bass_1").gain(0.6),
  // Instrument 4: 12-step counter anchors
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

More ready-made idioms: read `references/snippets.md`.

For richer examples with context (sampling, FX, arrangements, generative techniques, MIDI/motion/Hydra):
- `references/cookbook.md`

For full step-by-step composition builds:
- `references/walkthroughs.md`



[TODO: 1-2 sentences explaining what this skill enables]

## Structuring This Skill

[TODO: Choose the structure that best fits this skill's purpose. Common patterns:

**1. Workflow-Based** (best for sequential processes)
- Works well when there are clear step-by-step procedures
- Example: DOCX skill with "Workflow Decision Tree" → "Reading" → "Creating" → "Editing"
- Structure: ## Overview → ## Workflow Decision Tree → ## Step 1 → ## Step 2...

**2. Task-Based** (best for tool collections)
- Works well when the skill offers different operations/capabilities
- Example: PDF skill with "Quick Start" → "Merge PDFs" → "Split PDFs" → "Extract Text"
- Structure: ## Overview → ## Quick Start → ## Task Category 1 → ## Task Category 2...

**3. Reference/Guidelines** (best for standards or specifications)
- Works well for brand guidelines, coding standards, or requirements
- Example: Brand styling with "Brand Guidelines" → "Colors" → "Typography" → "Features"
- Structure: ## Overview → ## Guidelines → ## Specifications → ## Usage...

**4. Capabilities-Based** (best for integrated systems)
- Works well when the skill provides multiple interrelated features
- Example: Product Management with "Core Capabilities" → numbered capability list
- Structure: ## Overview → ## Core Capabilities → ### 1. Feature → ### 2. Feature...

Patterns can be mixed and matched as needed. Most skills combine patterns (e.g., start with task-based, add workflow for complex operations).

Delete this entire "Structuring This Skill" section when done - it's just guidance.]

## [TODO: Replace with the first main section based on chosen structure]

[TODO: Add content here. See examples in existing skills:
- Code samples for technical skills
- Decision trees for complex workflows
- Concrete examples with realistic user requests
- References to scripts/templates/references as needed]

## Resources

This skill includes example resource directories that demonstrate how to organize different types of bundled resources:

### scripts/
Executable code (Python/Bash/etc.) that can be run directly to perform specific operations.

**Examples from other skills:**
- PDF skill: `fill_fillable_fields.py`, `extract_form_field_info.py` - utilities for PDF manipulation
- DOCX skill: `document.py`, `utilities.py` - Python modules for document processing

**Appropriate for:** Python scripts, shell scripts, or any executable code that performs automation, data processing, or specific operations.

**Note:** Scripts may be executed without loading into context, but can still be read by Claude for patching or environment adjustments.

### references/
Documentation and reference material intended to be loaded into context to inform Claude's process and thinking.

**Examples from other skills:**
- Product management: `communication.md`, `context_building.md` - detailed workflow guides
- BigQuery: API reference documentation and query examples
- Finance: Schema documentation, company policies

**Appropriate for:** In-depth documentation, API references, database schemas, comprehensive guides, or any detailed information that Claude should reference while working.

### assets/
Files not intended to be loaded into context, but rather used within the output Claude produces.

**Examples from other skills:**
- Brand styling: PowerPoint template files (.pptx), logo files
- Frontend builder: HTML/React boilerplate project directories
- Typography: Font files (.ttf, .woff2)

**Appropriate for:** Templates, boilerplate code, document templates, images, icons, fonts, or any files meant to be copied or used in the final output.

---

**Any unneeded directories can be deleted.** Not every skill requires all three types of resources.
