---
name: performance
description: Diagnose and improve measured runtime, throughput, or memory bottlenecks in research training, inference, and data loading while checking numerical behavior.
---

# Performance

Follow the [shared coding guidance](../SKILL.md) and the active cluster's execution rules. Start from a concrete workload and performance symptom.

## Establish a baseline

Reuse existing timings, profiler traces, and benchmark commands when they answer the question. Otherwise choose the smallest representative measurement of the suspected bottleneck. Record the relevant hardware, precision, batch or shape, model/config, and input conditions.

Distinguish startup, compilation, and cache effects from steady-state execution. Account for asynchronous GPU work when timing it. Repeat enough to understand variability, and avoid claiming a speedup smaller than measurement noise.

## Change the measured bottleneck

Locate whether time or memory is spent in loading, transfer, computation, communication, or synchronization. Use that evidence to select a focused change. Reuse existing vectorized operations and runtime settings before adding custom kernels, caching systems, or new dependencies.

Preserve output and gradient semantics, sample coverage, and distributed behavior as applicable. If a speedup trades accuracy, precision, memory, or determinism, state the tradeoff rather than treating it as equivalent behavior.

## Validate without a benchmark campaign

Compare before and after under matched conditions. Pair the performance measurement with the smallest relevant numerical or behavioral check, using appropriate tolerances. A microbenchmark supports a claim about that operation; measure end-to-end behavior only when needed to establish the requested workload benefit.

Do not benchmark every shape, GPU, or configuration unless the optimization's scope requires it. Reuse a benchmark harness if one exists; a focused command and recorded conditions can suffice for a one-off investigation.

Stop when the requested improvement is established or measurements show the proposed change does not help. Report the measured difference, variability, correctness check, and relevant tradeoff. Do not present an unmeasured optimization as a demonstrated speedup.
