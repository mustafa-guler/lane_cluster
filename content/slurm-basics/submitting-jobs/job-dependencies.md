---
title: "Job Dependencies"
weight: 40
---
You may end up wanting to have jobs wait for other jobs before they execute, either to guarantee some preprocessing or conserve cluster resources.
This can be accomplished with the `-d` flag of `sbatch`.
The `-d` flag takes values of the format `type:job_id`.

| `-d` Type | Explanation |
| :----- | :------ |
| `after` | After job ID has *begun*, not ended |
| `afterany` | After job ID has *finished*, regardless of error status |
| `afterok` | After job ID has *finished* and *has not* exited with an error |
| `afternotok` | After job ID has *finished* and *has* exited with an error |
| `aftercorr` | After array task of job ID corresponding with this array job's array task has finished successfully. For example array task 1 of your new job will only run after array task 1 of your dependency job has finished without an error |
