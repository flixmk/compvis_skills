---
name: experiment-report
description: Report experiment status and results, compare runs, and assess claims against code, configs, logs, and artifacts while answering the user's specific questions.
---

# Experiment reporting

## Preserve the request

At the start, copy the user's reporting prompt verbatim into a temporary text file. Prefer an existing task-specific Codex working directory; otherwise use `mktemp` under an appropriate writable temporary directory. On MVL, bulky report artifacts belong on scratch; the small prompt record need not allocate compute. Do not create or modify global Codex configuration to store it.

Extract the requested questions, comparisons, and constraints. In the report, quote the relevant part of the user's prompt immediately before answering it, grouping closely related points when useful. Preserve any explicit output format. Answer every requested point, including those for which the evidence is incomplete.

Keep track of terms supplied by the user and terms introduced while writing. Reuse the user's terms; define necessary additions and explain what distinction they make. Include a glossary only when it helps the reader.

## Establish what actually ran

Start from the experiment README and existing reports, then verify consequential claims against the underlying artifacts. Collect only the provenance needed for this report:

- Run identity, code commit/dirty state when available, resolved config and overrides, checkpoint identity/step, and whether weights are EMA or non-EMA.
- Dataset/split, sample count, seeds, sampler and step count, guidance settings, and metric/reference implementation as relevant to the comparison.
- Job state, observed progress or completion, failures, and the exact log/result paths. Timestamp live status; distinguish planned, pending, running, failed, and completed work.

Do not infer completion from a launch command, or infer a result from an output directory's existence. Distinguish a missing measurement from a negative result. If a report and its raw evidence disagree, identify the discrepancy rather than silently selecting the preferred value.

## Present the result and its limits

For each question, provide the direct answer, the supporting evidence, and the limitation that changes its interpretation. Use a compact comparison table when several runs share comparable conditions. Include baseline and treatment values, metric direction, and useful absolute or relative differences with an explicit denominator.

- Compare like evaluation protocols. For diffusion, check checkpoint/EMA choice, seeds and sample count, sampling budget, guidance, and FID/KDD reference settings before ranking results. Mark confounded comparisons clearly.
- Report available uncertainty and repeat information; do not invent confidence intervals or treat one seed as evidence of robustness.
- Separate a measured effect from a proposed mechanism. Lower endpoint error alone does not prove why the model improved. For causal claims, report the relevant interventions, controls, and whether they support sufficiency, necessity, or only correlation.
- Carry forward failed controls and negative results that limit the conclusion. An observed correlation can be real while the proposed mechanism remains unsupported.
- Use the experiment's stated success criterion when one exists. The 90% mediation thresholds in some diffusion reports are experiment-specific, not a universal reporting standard.

State the decision supported by the evidence when requested. Describe a needed follow-up as proposed work unless it was actually performed; do not launch experiments merely to complete a report.

## Leave traceable output

Link the exact source tables/logs and viewable figures near the relevant claims. Use absolute paths for local artifacts in user-facing answers. Save a durable report when requested or when the existing experiment workflow requires one, using its established `reports/` or documentation location. Keep large tables and media on scratch and small provenance text in the repo as its rules permit.

Before delivering, check coverage against the saved prompt, verify quoted numbers and comparison settings, and ensure links resolve. No need to recreate a run registry if the experiment already has one.

## Local examples

- [Diffusion experiment framework](/export/home/ra63vex/dev/diffusion/EXPERIMENT.md): resolved configs, run provenance, explicit resume paths, and scratch artifacts.
- [Velocity formula report](/export/home/ra63vex/dev/diffusion/experiments/cross_token_valley/reports/velocity_formula_contract_v1.md): question, mathematical contract, run inputs, measurements, and caveats.
- [Transport-rank report](/export/home/ra63vex/dev/diffusion/experiments/cross_token_valley/reports/finite_time_transport_rank_results_v1.md): a positive observable alongside a rejected mediation hypothesis.

Use these as structural examples, not sources of current results for a different experiment.
