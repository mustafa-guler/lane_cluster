---
title: "A Simple sbatch Script"
weight: 10
---
Consider the following shell script.

```Shell
#!/bin/bash
# contents of example_script.sh

seq 1 10000 | shuf | sort -n
```
This prints numbers from 1 to 10000 (`seq`), randomly shuffles them (`shuf`), then attempts to resort them.
How could we submit this as a job?
The simplest answer is to just execute `sbatch` with this script as a parameter.
```Shell
sbatch -p pool1 -n 1 example_script.sh
```
This will request one task (`-n 1`) on the pool1 partition (`-p pool1`) and run our script on it.

How can we check the output? By default Slurm will write stdout and stderr to files named after the job ID.
This is not useful for us usually because it's unlikely you'll be able to remember what job ID was associated with each job.
We can use the following command instead.
```Shell
sbatch -p pool1 -n 1 -o ~/silly_sort.out -e ~/silly_sort.err example_script.sh
```
We have added paths that Slurm should write the stdout (`-o ~/silly_sort.out`) and stderr (`-e ~/silly_sort.err`) for the job.

What if we think this is too slow and we want to parallelize it. We can use the `--parallel` option for `sort`.
```bash
# contents of example_script.sh
#!/bin/bash

seq 1 10000 | shuf | sort -n --parallel=8
```
Now we can't just ask for a single task.
Each task by default is given a single CPU, but we want 8 CPUs for our one task.
We can use the following instead.
```Shell
sbatch -p pool1 -n 1 -c 8 -o ~/silly_sort.out -e ~/silly_sort.err example_script.sh
```
This requests 8 CPUs for each task (`-c 8`) in our job.

Now let's say we actually want to do our original silly sort multiple times for different sequences.
Our `example_script.sh` might now look like this.
```bash
# contents of example_script.sh
#!/bin/bash

seq 1 10000 | shuf | sort -n --parallel=8 > ~/silly_sorted1.txt
seq 1 5 100000 | shuf | sort -n --parallel=8 > ~/silly_sorted2.txt
seq 1 20 1000000 | shuf | sort -n --parallel=8 > ~/silly_sorted3.txt
```
As it stands this script has a problem.
It will wait for the first silly sort to finish, and then run the second and wait for it to finish, and then run the third.
We can do this in parallel by modifying the script as follows.
```bash
# contents of example_script.sh
#!/bin/bash

srun --exclusive -n 1 -c 8 seq 1 10000 | shuf | sort -n --parallel=8 > ~/silly_sorted1.txt &
srun --exclusive -n 1 -c 8 seq 1 5 100000 | shuf | sort -n --parallel=8 > ~/silly_sorted2.txt &
srun --exclusive -n 1 -c 8 seq 1 20 1000000 | shuf | sort -n --parallel=8 > ~/silly_sorted3.txt &
wait
```
Now each of our commands is run under `srun`.
This makes sure that our allocated resources are split among the tasks in our job.
The `-n` and `-c` flags are the same as in `sbatch`.
The new option `--exclusive` makes sure that we don't share resources across concurrent `srun` calls.
The `&` at the ends makes the normally blocking `srun` command run in the background and `wait` waits for all background processes to finish.
We can submit this new job with the following.
```
sbatch -p pool1 -n 3 -c 8 -o ~/silly_sort.out -e ~/silly_sort.err example_script.sh
```
Note that now we request 3 tasks with `-n`.
In general we only need to request as many tasks per job that we plan to run *in parallel*.
If we didn't want to run our three silly sorts in parallel and instead wanted to run them serially we could remove the
`&` and `wait` from our script and submit with
```Shell
sbatch -p pool1 -n 1 -c 8 -o ~/silly_sort.out -e ~/silly_sort.err example_script.sh
```

We have one more problem. What if our `sort` implementation is strange in that it always uses slightly less than a gigabyte of memory for each `--parallel`. We can use the `--mem-per-cpu` argument to guarantee that we have enough memory.
The script will now be
```bash
# contents of example_script.sh
#!/bin/bash

srun --exclusive -n 1 -c 8 --mem-per-cpu 1G seq 1 10000 | shuf | sort -n --parallel=8 > ~/silly_sorted1.txt &
srun --exclusive -n 1 -c 8 --mem-per-cpu 1G seq 1 5 100000 | shuf | sort -n --parallel=8 > ~/silly_sorted2.txt &
srun --exclusive -n 1 -c 8 --mem-per-cpu 1G seq 1 20 1000000 | shuf | sort -n --parallel=8 > ~/silly_sorted3.txt &
wait
```
and it can be submitted with
```Shell
sbatch -p pool1 -n 3 -c 8 --mem-per-cpu 1G -o ~/silly_sort.out -e ~/silly_sort.err example_script.sh
```

This command is starting to get ugly.
If we don't care about executing our script except on our cluster there is a way to make this cleaner.
```bash
# contents of example_script.sh
#!/bin/bash

#SBATCH -p pool1
#SBATCH -n 3
#SBATCH -c 8
#SBATCH --mem-per-cpu 1G
#SBATCH -o ~/silly_sort.out
#SBATCH -e ~/silly_sort.err

srun --exclusive -n 1 -c 8 --mem-per-cpu 1G seq 1 10000 | shuf | sort -n --parallel=8 > ~/silly_sorted1.txt &
srun --exclusive -n 1 -c 8 --mem-per-cpu 1G seq 1 5 100000 | shuf | sort -n --parallel=8 > ~/silly_sorted2.txt &
srun --exclusive -n 1 -c 8 --mem-per-cpu 1G seq 1 20 1000000 | shuf | sort -n --parallel=8 > ~/silly_sorted3.txt &
wait
```
Now we can simply submit with `sbatch example_script.sh`.
Lines that begin with `#SBATCH` will have their flags read from the script and passed automatically to the `sbatch` command.

