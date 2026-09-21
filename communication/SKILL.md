---
name: communication
description: Explain research and technical work in the user's terminology, choosing an appropriate level of detail for answers, experiment reports, and whiteboard walkthroughs.
---

# Communication

Lead with the answer to the user's actual question, then provide the evidence or explanation needed to assess it. Preserve their requested scope, format, comparison, and level of detail.

## Choose the relevant mode

- Use [explanations](explanations/SKILL.md) for quick, precise, or visual explanations of a concept, implementation, or result.
- Use [experiment reports](experiment_report/SKILL.md) to report runs, compare results, or assess what the available experiments establish.
- Use [whiteboard](whiteboard/SKILL.md) for a connected visual walkthrough of a mechanism, derivation, or data flow.

Read only the modes needed by the request. A report can contain an explanatory diagram without becoming a separate presentation.

## Shared conventions

- Keep the user's terminology and notation consistent. Introduce a new term only when it helps answer the question; define it at first use and connect it to the user's wording. Do not rename an observation to make it sound like an explanation.
- Distinguish what the implementation does, what was measured, and what is inferred. Attach uncertainty to the particular claim it limits.
- Cite the relevant code, config, log, result table, or artifact near the claim. A launcher establishes intended execution; a log or result establishes what actually happened.
- For comparisons, identify the baseline and what changed. State metric direction, units, and evaluation conditions where needed to interpret the result.
- Explain technical details in the order the reader needs them. Prefer connected prose; use tables for parallel comparisons and visuals when spatial or causal structure helps.
- If evidence is missing, say which part cannot be answered from the available material. Reporting or explaining existing work does not by itself request new training, sweeps, or evaluations.

The local diffusion reporting examples linked in the detailed skills illustrate these conventions; their historical conclusions are not facts to reuse in a new answer.
