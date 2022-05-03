---
title: "Array Jobs"
weight: 30
---
What if I want to perform the same computation on a very large number of inputs?
The Slurm solution to this is an array job.
An array job executes the same script many times.
It is specified with the `sbatch` flag `-a`. 
For each integer value of `-a` Slurm will create a distinct job and within that job set the environment variable `SLURM_ARRAY_TASK_ID` to the associated value of `-a`.

| Example `-a` | Explanation |
| :----------- | :---------- |
| `-a 1-100` | Run an array job with array values 1,2,3,...,100 |
| `-a 100-200` | Run an array job with array values 100,102,103,...,200 | 
| `-a 0-15:4` | Run an array job with array values 0,4,8,12 | 
| `-a 0-15:2` | Run an array job with array values 0,2,4,6,8,10,12,14 | 
| `-a 0-15%2` | Run an array job with array values 0,1,2,3,...,15 but only ever have 2 running simultaneously | 

There are limitations to array jobs.
The array indices cannot exceed the maximum array size configured on the Slurm cluster.
You can check this value with
```
scontrol show config | grep MaxArraySize
# output:
#
# MaxArraySize            = 1001
```
Note the name of this configuration value is misleading.
It would make you think `-a 1000-2000` would be acceptable since the array length is less than `1001`.
However, the way this configuration value is applied it actually restricts the maximum value of an index, not the array size.
A MaxArraySize of `1001` means your `-a` value cannot have an index that exceeds `1001`.

### A Common Array Paradigm
Say we have a data file for which we want to compute something from each line.
For example, we could have `data.smi` that contains a SMILES string on each line and we want to run `expensive_script` that accepts a SMILES string as a parameter and writes some computed value to stdout.
Let's make a job script.
```bash
# contents of array_example.sh
#!/bin/bash

#SBATCH -p pool1
#SBATCH -n 1
#SBATCH --mem 5G
#SBATCH -o ~/array_example_%a.out
#SBATCH -e ~/array_example_%a.err

# extract the smiles associated with the line in data.smi for our array task ID
smiles="$(sed -n "${SLURM_ARRAY_TASK_ID}p" data.smi)"

# run the script on that smiles
srun -n 1 --mem 5G expensive_script $smiles
```
There are a few new things here.

First, we have `%a` in the stdout and stderr files.
The value `%a` gets expanded by Slurm into the same value as `$SLURM_ARRAY_TASK_ID`.
These `%` formatting strings allow you to use Slurm variables in your Slurm parameters.
For more information see the `filename pattern` section of the `sbatch` man page.
Using `%a` we guarantee that each array task in array job has its logs written to separate files.

Next, we have this line:
```
smiles="$(sed -n "${SLURM_ARRAY_TASK_ID}p" data.smi)"
```
The command `sed -n "NUMBERp" FILE` prints a particular line number in a file.
In our case it pulls out the SMILES on the line of `data.smi` that is the same as our array task ID.
The `$()` captures this output and stores in the variable `smiles`.

We can submit our example script with `sbatch -a 1-100 array_example.sh` to process the first `100` lines of `data.smi`.
We can also use `sbatch -a 1-1000 array_example.sh` to process the first `1000` lines.
Remember, our `MaxArraySize` configuration is `1001`, so we can't actually have values of `-a` larger than this.
So what do we do if `data.smi` has more than `1000` lines?

Let's look at this modified script.
```bash
# contents of big_array_example.sh
#!/bin/bash

#SBATCH -p pool1
#SBATCH -n 1
#SBATCH --mem 5G
#SBATCH -o ~/array_example_%x_%a.out
#SBATCH -e ~/array_example_%x_%a.err

# determine the line number from the job name and task ID
line_num=$((SLURM_ARRAY_TASK_ID + $(echo $SLURM_JOB_NAME | cut -f2 -d_)))

# extract the smiles associated with the line in data.smi for our array task ID
smiles="$(sed -n "${line_num}p" data.smi)"

# run the script on that smiles
srun -n 1 --mem 5G expensive_script $smiles
```
The changes are now our logs contain the `%x` formatter, which expands to the job name specified via `sbatch` flag `-J`.
This job name is stored in the environment variable `SLURM_JOB_NAME` as well.
In this line
```
line_num=$((SLURM_ARRAY_TASK_ID + $(echo $SLURM_JOB_NAME | cut -f2 -d_)))
```
We add the array task ID to a number in the job name.
This is helpful because now we can do the following.
```
sbatch -J smilesprocessing_0 -a 1-1000 big_array_example.sh
sbatch -J smilesprocessing_1000 -a 1-1000 big_array_example.sh
sbatch -J smilesprocessing_2000 -a 1-1000 big_array_example.sh
```
We no longer violate `MaxArraySize` but we can process lines past line `1000` in `data.smi` by adding the array task ID to the part of the job name that is after the `_`.
The output for line `1015` in `data.smi` will be stored in `~/array_example_smilesprocesing_1000_15.out`.

We can run into a few new issues though.

First, there is maximum number of jobs each user can submit.
```bash
scontrol show config | grep MaxJobCount
# output:
#
# MaxJobCount             = 10000
```
You cannot submit more than `10000` jobs at once on Lane. Be careful of this.

Second, doing this will quickly flood queues and take control of all resources.
I've made this mistake, and it can be inconsiderate to every else.
There is a simple way around this.
```
sbatch -J smilesprocessing_0 -a 1-1000%50 big_array_example.sh
# previous submission had job ID 123
sbatch -J smilesprocessing_1000 -a 1-1000%50 -d afterany:123 big_array_example.sh
# previous submission had job ID 124
sbatch -J smilesprocessing_2000 -a 1-1000%50 -d afterany:124 big_array_example.sh
```
The `%50` means that only `50` of the array job's array tasks will run at the same time ever.
The `-d` flag specifies a job dependency. For more information see [Job Dependencies](https://gitlab.com/mguler/lane_cluster/-/wikis/Slurm-Basics#job-dependencies).
In this case the first `1000` lines will be processed `50` at a time.
Then, regardless of errors in the first `1000` lines the lines `1000-2000` will be processed `50` at a time.
However, this will only happen once the first array job has finished.
Using design pattern means you'll only use a fixed amount of the cluster's resources at a given time but also lets you set and forget these large array jobs.
