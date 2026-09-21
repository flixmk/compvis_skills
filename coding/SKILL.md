---
name: coding
description: Implement, debug, review, and optimize research code with scoped changes and proportionate validation. Use for coding work in the project's existing repositories.
---

# Coding

Understand the requested behavior and the code it touches before choosing a solution. Read the repository's instructions, trace the relevant execution path and callers, and reuse its existing components and conventions.

## Choose the relevant workflow

- Use [implementation](implementation/SKILL.md) for features, behavior changes, and refactoring.
- Use [debugging](debugging/SKILL.md) for failures, incorrect results, and regressions.
- Use [review](review/SKILL.md) to assess correctness, unintended changes, and unnecessary complexity.
- Use [performance](performance/SKILL.md) for measured runtime, throughput, or memory bottlenecks.

Read only the workflows needed. Use the relevant [compute guidance](../compute/SKILL.md) and cluster skill for execution; implementation work does not by itself call for a training run or research sweep.

## Shared expectations

Prefer the smallest complete change supported by the actual requirements. Search for existing helpers before adding code; use the standard library or existing dependencies before introducing another dependency. Add abstractions when current uses justify them.

Preserve contracts that matter to the affected path: tensor shape, dtype, device, normalization, gradients, data sharding, distributed collectives, and checkpoint semantics. Inspect which apply rather than treating every edit as a distributed-systems redesign. Make intentional behavior changes explicit.

Keep unrelated edits out of the change. Follow the repo's configuration and ownership boundaries, and preserve research parameters needed for the requested comparison. Do not hide bugs behind silent fallbacks.

## Avoid overtesting

Choose validation by the behavior at risk and the cost of being wrong. Before adding a test, identify the concrete failure it would catch and inspect existing coverage.

- Run the relevant existing check first when it can answer the question. Extend an existing test when needed instead of duplicating its coverage in a new file.
- For nontrivial new or changed logic, leave a runnable check of the observable behavior or invariant. Existing coverage can satisfy this; otherwise a small regression case or assertion-based check is often enough. Do not create a test for every function.
- Documentation, formatting, mechanical renames, and other low-impact changes usually need inspection and the applicable lightweight checks. Do not add tests that merely assert wording, source layout, private call sequences, or a restatement of the implementation.
- Use a small deterministic input for numerical or data-path checks. Cover the relevant boundary or failure mode; avoid an exhaustive product of devices, dtypes, seeds, shapes, and configs without evidence that those combinations matter.
- Add an integration or distributed check when the risk crosses that boundary. A CPU unit test cannot establish multi-rank correctness, checkpoint recovery, or actual GPU performance; equally, an unrelated edit does not justify those expensive runs.
- Reuse the installed test tooling. Do not add a framework, fixture hierarchy, mock infrastructure, or benchmark suite for a one-off check. Mock only boundaries needed to exercise the real behavior.
- Complete required repository checks. Broaden testing when a failure, shared-contract change, or unresolved risk gives a reason; do not run the full suite by habit.

Once the relevant checks pass, stop. Repeat them only after a pertinent change, an unreliable measurement, or new evidence. Do not run training, large evaluations, or sweeps merely to make validation feel thorough. Avoid deleting useful existing tests under the banner of reducing testing.

Report what was checked, the outcome, and any material gap in what that check establishes. If execution is unavailable, distinguish an unverified change from a verified one without constructing unrelated substitute tests.
