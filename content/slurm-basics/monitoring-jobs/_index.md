---
title: "Monitoring Jobs"
weight: 70
---
After you have submitted your jobs you likely want to monitor their status.
This is done with the `squeue` command.
Executed without any flags it will list all queued jobs across the whole cluster.
Generally, you only will want to look at your jobs.
You can do this with `squeue -u $(whoami)`.
Let's look at my jobs.
```bash
squeue -u mguler
# output:
#
#            JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
#         349613_1 moh1,pool train_gn   mguler PD       0:00      1 (DependencyNeverSatisfied)
#         349613_2 moh1,pool train_gn   mguler PD       0:00      1 (DependencyNeverSatisfied)
#         349613_3 moh1,pool train_gn   mguler PD       0:00      1 (DependencyNeverSatisfied)
#         349613_4 moh1,pool train_gn   mguler PD       0:00      1 (DependencyNeverSatisfied)
#         349613_5 moh1,pool train_gn   mguler PD       0:00      1 (DependencyNeverSatisfied)
#           349388  moh2-gpu train_zi   mguler  R   15:24:51      1 compute-4-18
```
We can see I have 6 jobs. The first five are part of an array job, as we can see by the `_` in the `JOBID`.
They were submitted to both `moh1` and `pool1` as shown by `PARTITION` and requested `1` node as shown by `NODES`.
They are all in the `PD` or pending state because their dependency was never satisfied (columns `ST` and `NODELIST(REASON)`).
This happens when you use the `afterok` dependency and your dependency exits with an error.
The 6th job is currently in the `R` or running state and is running on node `compute-4-18` on `moh2-gpu`.
We can see it has been running for 15 hours, 24 minutes, and 51 seconds in the `TIME` column.
The most common job states you'll see are `PD` for pending, `R` for running, and `CG` for completing.

## External Resources
- [`squeue` man page](https://slurm.schedmd.com/squeue.html)
