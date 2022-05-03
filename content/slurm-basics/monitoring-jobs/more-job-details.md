---
title: "More Job Details"
weight: 0
---

We have some options to get more details about jobs.
Looking at my `train_zinc` job we can break down status by task with
```bash
sacct -j 349388
# output:
#
#        JobID    JobName  Partition    Account  AllocCPUS      State ExitCode
# ------------ ---------- ---------- ---------- ---------- ---------- --------
# 349388       train_zin+   moh2-gpu      local          1    RUNNING      0:0
# 349388.batch      batch                 local          1    RUNNING      0:0
# 349388.exte+     extern                 local          1    RUNNING      0:0
# 349388.1         python                 local          1    RUNNING      0:0
```
and get all the job's configuration with
```bash
scontrol show job 349388
# output:
#
# JobId=349388 JobName=train_zinc.sh
#    UserId=mguler(20931) GroupId=mguler(20931) MCS_label=N/A
#    Priority=1207 Nice=0 Account=local QOS=normal WCKey=*default
#    JobState=RUNNING Reason=None Dependency=(null)
#    Requeue=1 Restarts=2 BatchFlag=2 Reboot=0 ExitCode=0:0
#    RunTime=15:35:03 TimeLimit=1-00:00:00 TimeMin=N/A
#    SubmitTime=2020-11-04T20:36:07 EligibleTime=2020-11-04T20:38:08
#    AccrueTime=2020-11-04T20:38:08
#    StartTime=2020-11-04T20:38:36 EndTime=2020-11-05T20:38:36 Deadline=N/A
#    SuspendTime=None SecsPreSuspend=0 LastSchedEval=2020-11-04T20:38:36
#    Partition=moh2-gpu AllocNode:Sid=lanec1.compbio.cs.cmu.edu:18843
#    ReqNodeList=(null) ExcNodeList=(null)
#    NodeList=compute-4-18
#    BatchHost=compute-4-18
#    NumNodes=1 NumCPUs=1 NumTasks=1 CPUs/Task=1 ReqB:S:C:T=0:0:*:*
#    TRES=cpu=1,mem=100G,node=1,billing=1,gres/gpu=1
#    Socks/Node=* NtasksPerN:B:S:C=0:0:*:* CoreSpec=*
#    MinCPUsNode=1 MinMemoryNode=100G MinTmpDiskNode=0
#    Features=(null) DelayBoot=00:00:00
#    OverSubscribe=OK Contiguous=0 Licenses=(null) Network=(null)
#    Command=/projects/mohimanilab/mustafa/tools/constrained-graph-variational-autoencoder/train_zinc.sh
#    WorkDir=/projects/mohimanilab/mustafa/tools/constrained-graph-variational-autoencoder
#    StdErr=/projects/mohimanilab/mustafa/tools/constrained-graph-variational-autoencoder/train_zinc.err
#    StdIn=/dev/null
#    StdOut=/projects/mohimanilab/mustafa/tools/constrained-graph-variational-autoencoder/train_zinc.out
#    Power=
#    TresPerNode=gpu:1
```

We have seem `scontrol` a few times now. It is generally used to display configuration information at various granularities (cluster-level, node-level, job-level).
We have not seen `sacct` yet. It is used to show accounting data for jobs and their steps/task.

## External Resources
- [`squeue` man page](https://slurm.schedmd.com/squeue.html)
- [`scontrol` man page](https://slurm.schedmd.com/scontrol.html)
- [`sacct` man page](https://slurm.schedmd.com/sacct.html)

