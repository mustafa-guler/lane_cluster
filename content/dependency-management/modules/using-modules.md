---
title: "Using Modules"
weight: 30
---
Once you have added a modulefile directory with `module use` you can load modules with `module load` and unload them with `module unload`.
At any point you can use `module list` to see what modules you have active.
Additionally, you can use `module purge` to unload all loaded modules.
You can add `module load` commands to the beginning of jobs scripts to use modules in your jobs.
For more information on submitting jobs see [Submitting Jobs](/lane-cluster/slurm-basics/submitting-jobs).
