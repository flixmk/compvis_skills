---
name: compute
description: Identify and work with the Helma, Jupiter, and MVL compute environments. Use for cluster jobs and environment-specific commands.
---

Identify the cluster before using cluster-specific commands:

1. Run `scontrol show config` and read `ClusterName`; a hostname such as `login1` is ambiguous.
2. Treat `ClusterName = compvis` as MVL.
3. If Slurm is unavailable, use these filesystem fingerprints:
   - MVL: `/export/home`, `/export/scratch`, or `/export/group`
   - Helma: `/hnvme`
   - Jupiter: `/e/project1` or `/e/scratch`; its GPU scripts use the `booster` partition

If the signals conflict, ask the user instead of guessing.
