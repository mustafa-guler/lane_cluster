---
title: "Useful sbatch Flags"
weight: 20
---
In the following table `X` is an integer and `S` is a string.

| Flag | Value Format | Description |
| --- | ---------- | ---------- |
| `-p` | `S` | Partition that job should be submitted to. Can be a comma-separated list if you aren't concerned with forcing a particular partition | 
| `-n` | `X` | Number of *concurrent* tasks you will run for this job. Tasks can be allocated from within your job script with `srun` |
| `-c` | `X` | Number of CPUs each task will use for this job |
| `--mem-per-cpu` | `X[KMGT]` for KB,MB,GB,TB respectively | Amount of memory per CPU this job requires |
| `--mem` | `X[KMGT]` for KB,MB,GB,TB respectively | Amount of memory this job requires *total*, not per CPU |
| `-t` | `X-XX:XX:XX` | Sets a maximum running time for your job. Provided value format is in `days-hours:minutes:seconds`. For more formatting see `--time` entry in `sbatch` man page |
| `-J` | `S` | Name of your job |
| `-o` | `S`, filepath | Path where the stdout of this job should be written |
| `-e` | `S`, filepath | Path where the stderr of this job should be written |

For `srun` these are the same. The additional flag for `srun` that isn't for `sbatch` that should be kept in mind is `--exclusive`.

## External Resources
- [`sbatch` man Page](https://slurm.schedmd.com/sbatch.html)
- [`srun` man Page](https://slurm.schedmd.com/srun.html)
