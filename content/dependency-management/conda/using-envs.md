---
title: "Using Conda Environments"
weight: 20
---
After you have [created an environment](/dependency-management/conda/creating-envs) you need to activate it before you can access its contents.
If you are running an interactive job you can do this via `conda activate ENV_NAME`. 
However, this does not always work as expected when it is part of a job submitting with `sbatch` (See [Submitting Jobs](/lane-cluster/slurm-basics/submitting-jobs) for more information on job submission).
You can work around this by activating a `conda` environment with the command
```bash
source activate ENV_NAME
```

Once you have activated a `conda` environment via either method you have access to everything you installed in there.
You can install more packages with the `conda install` command.
Specific versions of package names can be specific with an `=` after the package name.
It's recommended that if you know all the packages you want beforehand you group the installations into one install command.
This is because `conda` contains a SAT solver to pick package versions and if all the packages are given at once it can decide quicker.

You can also install packages from pip in a `conda` environment. Make sure you activate your `conda` environment first, then use pip as you normally would. Generally pip dependencies are not checked for version compatibility, so if possible use the `conda` package for the library you're interested in.
