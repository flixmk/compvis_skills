---
name: whiteboard
description: Build a visual walkthrough of a research mechanism, mathematical derivation, or code/data flow when the user asks for a whiteboard explanation or needs a connected sequence of diagrams.
---

# Whiteboard walkthroughs

Build the walkthrough around the question the user wants to understand. Use their vocabulary and mathematical conventions. Inspect the relevant code or result artifacts before drawing a claimed implementation or measured behavior.

## Build the story

Choose a connected sequence suited to the question:

1. Establish the objects, starting state, and baseline.
2. Show the operation, intervention, or change, labeling what is held fixed.
3. Show how that change propagates through the relevant steps.
4. Connect the consequence to the original question and identify what the evidence leaves unresolved.

Use as few panels as the explanation needs; a single diagram is enough for a simple flow. Give each panel one question or takeaway. Reuse symbols, colors, sample identities, and spatial positions so the reader can follow the same object across panels.

For diffusion, mark time direction and distinguish the state, model prediction, solver update, and final metric. Connect equations to the corresponding arrows or transformations. Do not draw a causal arrow from a correlation unless it is explicitly labeled as a hypothesis.

## Choose a usable visual

- Use Mermaid or a simple vector diagram for compact architecture and data-flow explanations.
- Use standard plotting tools for empirical curves, scientific figures, and exportable results. Reuse existing summaries before recomputing expensive tensors.
- Use an interactive view when a time slider, parameter change, or linked selection reveals behavior a static image would hide. Keep the default view understandable and label controls with their scientific meaning.

Label axes, units, color meanings, and baselines. Use comparable scales for comparable panels and mark any projection or normalization that changes interpretation. Do not rely on color alone; add labels or line styles. Distinguish measured data from toy examples and schematic geometry, including in captions.

## Deliver and check

Show the visual with a short guided reading, not just a path to an unseen file. Supply a viewable/exportable artifact when the user needs to keep or share it. Use absolute local paths in user-facing links and image embeds. Keep bulky generated files on scratch according to the active repo's storage rules, and perform compute-heavy generation inside an allocation.

Inspect the rendered output for legible labels, clipping, arrow direction, consistent legends, and agreement between equations and graphics. Check interactive controls if present. Explain the main takeaway and the limitation needed to avoid overreading the picture.

## Local example

The [baseline-relative visual story](/export/home/ra63vex/dev/causal_tread/experiments_mlrc/representation/rep_far_plus_lowfreq_visual_story_v1/README.md) organizes existing summaries into successive questions about family comparisons, synergy, transport versus reuse, spatial patterns, and variation across samples. Reuse the pattern of one question per panel; do not assume its five-panel layout or historical claims fit another task.
