---
name: explanations
description: Give quick, precise, or visual explanations of research concepts, diffusion code, and experimental findings, preserving the user's terminology and requested depth.
---

# Explanations

Answer the exact question first. Choose the user's requested mode; otherwise use the shortest explanation that resolves the question. Keep their terms and notation, defining new ones only when needed.

## Quick

Give the answer and the minimum reason needed to make it clear. A small example can resolve an ambiguity; do not append a survey, derivation, or experiment plan unless requested. If the answer depends on an unresolved condition, state that condition directly.

## Precise

Start with the claim, define the objects and assumptions it depends on, then give a checkable derivation or code-based explanation. Provide the technical rationale and evidence needed to verify the answer.

For implementation questions, trace the actual config and execution path to the relevant operation. Connect symbols to code variables and distinguish mathematical intent from implemented behavior. Mention tensor shapes, normalization, boundary cases, or distributed behavior when they affect the answer.

For diffusion and flow-matching explanations:

- Verify the repo's time convention, velocity/score/noise parameterization, integration direction, and conditioning convention before writing equations. Different forks can use different definitions.
- Distinguish a local vector-field quantity, a finite sampler trajectory, and an endpoint evaluation metric. Improvement at one level does not establish improvement or causation at another.
- Name the baseline or intervention in the user's terms. A description of a residual's sign or frequency content is not by itself a mechanism explaining a benchmark gain.

Separate identities, approximations, observations, and hypotheses. State the condition under which a conclusion holds instead of obscuring it with general caveats.

## Visual

Produce a plot, diagram, or interactive demonstration that directly explains the requested point. Use [whiteboard](../whiteboard/SKILL.md) when the user wants to explore the underlying research ideas or mathematical questions together; that mode does not require a visual.

Prefer an existing relevant artifact when it already answers the question. Use a compact diagram for structure, a measured plot for empirical comparisons, and interaction when changing a parameter or moving through time teaches something useful. Clearly label synthetic or schematic examples so they cannot be mistaken for experiment results.

Define axes, units, legends, and the takeaway. Keep scales and sample identities consistent across comparisons, and provide a short text explanation alongside the visual. Put generated heavy artifacts on the active cluster's scratch storage; follow its allocation rules for rendering or data processing.

## Evidence and example

Link the code or artifact that supports the key claim. State when an explanation is limited by unavailable code, data, or measurements.

The [diffusion mechanism conclusion](/export/home/ra63vex/dev/diffusion/experiments/cross_token_valley/reports/static_fm_mechanism_conclusion_v1.md) illustrates why an endpoint residual description and a demonstrated flow mechanism must be distinguished. Preserve that reasoning discipline without importing the experiment's terminology or historical conclusion into unrelated answers.
