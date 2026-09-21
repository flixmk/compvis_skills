---
name: mvl
description: Run and inspect work on the MVL Slurm cluster, including diffusion launchers, Conda environments, scratch storage, checkpoints, and job diagnostics. Use after identifying ClusterName compvis.
---

# MVL compute

## Identify the environment and checkout

Run `scontrol show config` and read `ClusterName`: `compvis` identifies MVL; `login1` alone does not. Use `sinfo -h -o '%P %l %G'` to inspect current partitions, time limits, and GPU request types. A100 and H200 are present in the inspected launchers; check availability and account access before choosing one.

Read the target checkout's `AGENTS.md`, experiment README, and launcher before running anything. The repositories under `/export/home/ra63vex/dev/diffusion*` differ in configs, environments, and execution rules. A script copied from a sibling checkout is evidence, not necessarily a working command for this one.

## Allocate before executing workloads

The login node is shared. Use it for lightweight file inspection, Git, edits, and scheduler queries. Run training, evaluation, tensor/data loading, project tests, and heavy installs inside a Slurm compute allocation. A `SLURM_JOB_ID` in the environment alone does not prove that the current shell is on the allocated node.

For a short single-GPU interactive session:

```bash
salloc -p a100 --gres=gpu:a100:1 -t 00:30:00
# If salloc leaves the shell on the login node, enter the allocation:
srun --pty bash
hostname
scontrol show job "$SLURM_JOB_ID"
```

Compare the hostname with the job's allocated nodes before running project code. For H200, use the matching `-p h200 --gres=gpu:h200:1` request. Request only the resources and time the task needs.

The inspected diffusion instructions specify a **24 GPU-hour per-job budget**: total allocated GPUs across all nodes × walltime in hours must be at most 24. Thus 1 GPU permits 24 hours, 2 permit 12, and 4 permit 6. Treat this as the documented project budget, even when `sinfo` reports `infinite`; it does not establish every account's effective QoS limits. Respect tighter current limits and explicit task budgets.

Prefer batch jobs for long work. `diffusion/AGENTS.md` and `diffusion_imagenet_disentangled_guidance/AGENTS.md` additionally require running launch scripts and `sbatch` from a compute session; follow that rule when working in those checkouts. Inspect other checkouts' rules rather than assuming this is a universal Slurm requirement.

## Activate the existing environment

The MVL diffusion launchers use this shared installation and environment by default; preserve a launcher's documented overrides and verify the source file is readable:

```bash
export BATCH_CONDA_SH="${BATCH_CONDA_SH:-/export/scratch/ra63ral/miniconda3/etc/profile.d/conda.sh}"
export BATCH_CONDA_ENV="${BATCH_CONDA_ENV:-pytorch2.8_cu128}"
conda deactivate 2>/dev/null || true
source "$BATCH_CONDA_SH"
conda activate "$BATCH_CONDA_ENV"
export PYTHONNOUSERSITE=1
```

When inserting this into a script that enables `set -u`, follow the existing launchers' `set +u` / `set -u` bracket around Conda activation. Do not run `conda init` on every job or modify the shared environment as a routine fix. Inspect the interpreter and imports inside the allocation when diagnosing an environment mismatch.

## Resolve storage from the actual repo

Keep checkpoints, dataset caches, tensors, feature banks, generated plots/media, and large exports on scratch. Keep code, configs, and small provenance records in the checkout. Use ignored symlinks when code expects `outputs/`, `artifacts/`, or `cache/` beside the code; inspect existing destinations before changing links and never overwrite real directories to make a link.

Common locations in the inspected repositories:

| Role | Existing location |
| --- | --- |
| User checkouts | `/export/home/ra63vex/dev/` |
| User scratch | `/export/scratch/ra63vex/` |
| ImageNet latent shards | `/export/group/datasets/ILSVRC/imagenet256_latents_wds` |
| FID reference | `/export/scratch/ra63vex/imgnet_eval_stats/VIRTUAL_imagenet256_labeled.npz` |
| Inception weights | `/export/scratch/ra63vex/imgnet_eval_stats/inception-2015-12-05.pkl` |
| KDD reference bank | `/export/scratch/ra63vex/diffusion_imagenet_files/splitmeanflow/kdd_val.pt` |
| Clean FM checkpoints | `/export/scratch/ra63vex/diffusion_imagenet_clean_checkpoints/normal` |
| Clean split-mean-flow EMA checkpoints | `/export/scratch/ra63vex/diffusion_imagenet_clean_checkpoints/splitmeanflow_ema` |

These are account/project defaults, not guarantees of access or suitability. Check the requested dataset, checkpoint, and feature-bank metadata; do not substitute an unrelated available file.

In `/export/home/ra63vex/dev/diffusion`:

- `diffusion/utils/cluster_paths.py` and `configs/paths/cluster.yaml` define the paths. MVL's profile is **`default`**, selected explicitly with `DIFFUSION_PATH_PROFILE=default`; this resolver does not accept `mvl`.
- Individual path overrides use `DIFFUSION_PATH_<UPPERCASE_KEY>`, such as `DIFFUSION_PATH_OUTPUT_ROOT`. Preserve the configured reference feature extractor together with its feature bank.
- `scripts/setup_scratch_links.sh` supports `DIFFUSION_SCRATCH_ROOT` and creates scratch-backed links. Its default output link points into `/export/scratch/${USER}/diffusion/outputs`, while the inspected resolver's `output_root` points to `/export/scratch/ra63vex/diffusion_imagenet_tread_explore_ca`. Changing the links does **not** change Hydra's resolved output path; check both.

Some forks instead have `configs/cluster/mvl.yaml` with `${paths.*}` entries and `${oc.env:TMP}`. Inspect how their root config selects that group before using `cluster=mvl`; ensure the resolved temporary directory is writable and scratch-backed. Do not mix the two configuration schemes.

## Launch, monitor, and resume

Reuse `scripts/mvl/` for shared training launchers and `experiments/<slug>/slurm/launch/` for experiment launchers. Inspect the script's command, resource request, output paths, and environment overrides. Work from the expected checkout root and create its log directory before submission: Slurm opens `--output` and `--error` paths before the script runs.

In the main diffusion repo, `scripts/mvl/setup.sh` defines `launch`, selects Python or torchrun, coordinates Slurm ranks, and forwards checkpoint signals. Preserve that wrapper and any `#SBATCH --signal=USR1@600` behavior instead of constructing a competing distributed launch. `scripts/mvl/fm_img_imagenet_smoke.sh` is an existing smoke example; inspect its configured 1,000 steps and overrides before deciding whether it fits the task's budget.

After an authorized submission, record the returned job ID, exact command/config overrides, commit and dirty state, requested resources, and resolved output/log paths. Check status with:

```bash
squeue -u "$USER"
scontrol show job <job-id>
sacct -j <job-id> --format=JobID,State,ExitCode,Elapsed,AllocTRES
```

Replace `<job-id>` with the returned ID. Inspect pending reasons and the relevant log tail; a submitted job is not a completed result, and an exited job still needs its expected outputs checked. If a launch fails, diagnose the failure before resubmitting; avoid duplicate active jobs.

For reproducible resume, use the experiment's documented command and an explicit checkpoint step. Where supported, diffusion uses `checkpointing.initial_load_path=.../checkpoints/step-1000` for distributed checkpoints and `scripts/export_checkpoint.py` for inference export. Check the actual checkpoint format and whether the requested operation is resume, weights-only initialization, or inference; these are different operations.

Release task-owned interactive allocations when finished. Cancel only the jobs identified by the task, preserving unrelated work.

## Source anchors

Grounded in these server files, inspected on 2026-09-21; re-read them when the checkout changes:

- [Diffusion operational instructions](/export/home/ra63vex/dev/diffusion/AGENTS.md) and [experiment storage/provenance](/export/home/ra63vex/dev/diffusion/EXPERIMENT.md).
- [MVL launch wrapper](/export/home/ra63vex/dev/diffusion/scripts/mvl/setup.sh), [batch example](/export/home/ra63vex/dev/diffusion/scripts/mvl/example.sh), and [experiment launcher](/export/home/ra63vex/dev/diffusion/experiments/cross_token_valley/slurm/launch/recovery_mvl.sh).
- [Path resolver](/export/home/ra63vex/dev/diffusion/diffusion/utils/cluster_paths.py), [scratch-link helper](/export/home/ra63vex/dev/diffusion/scripts/setup_scratch_links.sh), and [fork MVL config](/export/home/ra63vex/dev/diffusion_imagenet_disentangled_guidance/configs/cluster/mvl.yaml).

These source paths are local provenance; the instructions above remain usable when those checkouts are unavailable.
