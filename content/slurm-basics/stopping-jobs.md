---
title: "Stopping Jobs"
weight: 80
---
If you want to stop your job first find your job ID with `squeue` then run `scancel JOBID`.
For array jobs using just the job ID before the underscore you will cancel all array tasks in the array job.
Using the underscore you will only cancel a single array step.

## External Resources
- [`scancel` man page](https://slurm.schedmd.com/scancel.html)
