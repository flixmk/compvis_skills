---
name: review
description: Review research-code changes for correctness, unintended behavior, contract violations, and unnecessary complexity, prioritizing concrete actionable findings.
---

# Code review

Follow the [shared coding guidance](../SKILL.md). Assess the requested change in context and scale the review to its risk.

## Trace the impact

Read the diff, the surrounding implementation, and relevant callers or configs. Establish the intended behavior before judging the design. Focus on paths the change can affect; expand the scope when dependencies reveal a concrete concern.

Check applicable contracts such as tensor semantics, gradient flow, data sharding, synchronization, checkpoint loading, and configuration defaults. Look for duplicated behavior, unused flexibility, and abstractions whose simpler replacement is clear.

## Use evidence proportionately

Inspect existing validation before asking for more. A missing new test is not automatically a defect: identify the untested behavior, a plausible failure, and why current coverage would miss it.

Use a targeted reproduction when a suspected bug needs confirmation. Avoid running training or constructing a broad test suite for a review. Distinguish a confirmed issue from a question that depends on unavailable runtime evidence.

Do not request tests for private implementation details, coverage percentages, or hypothetical configurations outside the supported contract. Require stronger evidence when the change affects a consequential boundary, such as distributed execution or checkpoint integrity.

## Report actionable findings

Lead with findings ordered by impact. For each, give the location, triggering condition, consequence, and a concise correction when clear. Separate correctness issues from optional simplifications; avoid style findings already handled by repository tooling.

If there are no actionable findings, say so and note only meaningful validation limitations. Reviewing a change does not require editing it unless the user requests fixes.
