---
name: implementation
description: Implement agreed features, behavior changes, and refactors in an existing research codebase while preserving its relevant contracts and keeping the change small.
---

# Implementation

Follow the [shared coding guidance](../SKILL.md), including proportionate validation. Start from the behavior the user wants and the existing path that owns it.

## Understand the change

Read the entrypoint, configuration, affected functions, and their callers. Identify which current behavior should remain and which behavior the request intentionally changes. For a refactor, preserve observable behavior unless the user also requests a semantic change.

Resolve routine choices from the codebase. Ask only when missing information materially changes the required behavior; do not turn implementation into an unsolicited research-design exercise.

## Make the smallest complete change

Reuse a suitable helper, type, config pattern, or dependency already present. Put shared behavior where affected callers naturally converge. Avoid fixing one caller while leaving equivalent callers inconsistent.

Keep stable infrastructure and experiment-specific code in their established locations. Preserve tensor and distributed contracts along the changed path, and retain configuration knobs needed by the actual task. Avoid speculative extension points, compatibility layers, and unrelated cleanup.

## Validate and deliver

Use an existing relevant check, or add the smallest case that would detect the changed behavior being wrong. For a refactor, compare meaningful outputs or invariants rather than testing the new internal arrangement. Add gradient or numerical checks when the changed computation makes them relevant.

Run applicable formatting and required checks, then inspect the diff for accidental scope changes. Stop when the relevant evidence is sufficient. Explain what changed, why, and what the checks establish; identify an intentional compatibility change when present.
