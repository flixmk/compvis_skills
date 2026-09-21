---
name: debugging
description: Diagnose failures, incorrect results, and regressions in research code by reproducing the symptom, tracing the root cause, and verifying a focused fix.
---

# Debugging

Follow the [shared coding guidance](../SKILL.md). Establish what failed and where before editing code.

## Narrow the failure

Read the exact error or unexpected result together with the command, config, environment, and relevant inputs. Separate code defects from configuration mistakes, missing data, environment mismatches, and scheduler failures. Use available logs and artifacts before rerunning expensive work.

Reduce the failure to the smallest useful reproduction. Keep conditions essential to the bug, such as rank count, precision, gradient tracking, or checkpoint state. A simpler case that removes the failure is not yet a reproduction.

Form a concrete hypothesis and choose a check that can distinguish it from plausible alternatives. Change one relevant factor at a time; do not accumulate speculative fixes or rerun a failing job unchanged without a reason.

## Fix the cause

Trace the failing function's callers and related paths. Fix the violated assumption at the appropriate shared boundary, preserving valid behavior. Use explicit errors for invalid inputs where appropriate; avoid broad exception handling, silent retries, or fallback values that conceal the defect.

## Verify the repair

Where practical, use a small regression case that fails before the fix and passes afterward. Reuse or extend existing coverage; one reproduction plus the directly affected checks often suffices. Expand only if the root cause affects a wider contract.

For numerical failures, verify the relevant values or gradients, not merely the absence of an exception. For infrastructure failures, verify the repaired environment or job state without inventing a code patch.

Report the cause, the fix, and the evidence. Stop after the failure is resolved and relevant checks pass; separate any remaining uncertainty from the repaired issue.
