---
title: "sbatch vs srun"
weight: 0
---
These two commands take almost the exact same parameters.
The difference is `sbatch` submits jobs to the Slurm scheduler, to be run when requested resources become available.
Running the same job directly with `srun` will actually run synchronously.
In other words, you will have to sit and wait while the job runs and logging out of the cluster will likely terminate your job.
It is generally not recommended to directly execute jobs with `srun`, one exception can be see in [Interactive Sessions](https://gitlab.com/mguler/lane_cluster/-/wikis/Slurm-Basics#interactive-sessions)

## External Resources
- [Useful distinction between `srun` and `sbatch`](https://stackoverflow.com/questions/43767866/slurm-srun-vs-sbatch-and-their-parameters)

