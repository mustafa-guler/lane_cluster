# Reading This Document

[Conda](https://gitlab.com/mguler/lane_cluster/-/wikis/Dependency-Management#conda) contains information about installing `conda`, creating `conda` environments, activating `conda` environments, and transferring `conda` environments across machines.

# Conda

`conda` is a python-oriented environment manager.
While it is primarily geared towards python it can be used for many things.
`conda` packages are pre-compiled for a particular system, so you will not need to deal with compiling your own dependencies.
`conda` environments sandbox dependencies from each other so different environments can exist and be used without polluting the global package space.

__External Resources__:
- [`conda` User Guide](https://docs.conda.io/projects/conda/en/latest/user-guide/index.html)


## Installing Conda

You may already have `conda` installed under a shared FS for your lab. Check if this is the case before proceeding.
If you use this installation please make sure you do not modify existing environments, as it could break other users' workflows.

To install your own copy go to the [Miniconda installation page](https://docs.conda.io/en/latest/miniconda.html) and copy the link to the latest linux installer.
As of writing this it is for Python 3.8.
Then run
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```
and follow the prompts.
Be sure that `conda init` was run.
You can do whatever you like to your own `conda` installation's environments.

__External Resources__:
- [Miniconda Installation Page](https://docs.conda.io/en/latest/miniconda.html)

## Creating Conda Environments

You can create `conda` environments with the `conda create` command.
For more information about the last example see [Transferring Conda Environments](https://gitlab.com/mguler/lane_cluster/-/wikis/Dependency-Management#transferring-conda-environments).

```bash
# create an environment named py27 that contains python 2.7
conda create -n py27 python=2.7

# create an environment named rdkit that contains a python 3 version and compatible rdkit installation
conda create -n rdkit -c conda-forge python=3 rdkit

# copy the rdkit environment to another environment named new_rdkit
conda create -n new_rdkit --clone rdkit

# create environment from environment.yaml
conda create --file environment.yaml
```

| Useful Options | Description |
| :---- | :----- |
| `-n` | Name of conda environment to create |
| `-c` | `conda` channel to use. Common useful ones are `conda-forge` for generic things not in the base `conda` repository and `bioconda` for comp bio software | 
| `--file` | Path to environment.yaml file described in [Transferring Conda Environments](https://gitlab.com/mguler/lane_cluster/-/wikis/Dependency-Management#transferring-conda-environments) |
| `--clone` | Name of `conda` environment to copy for this new environment |

__External Resources__:
- [`conda-forge`](https://conda-forge.org/)
- [`bioconda`](https://bioconda.github.io/)
- [anaconda package repository](https://anaconda.org/anaconda/repo)

## Using Conda Environments

After you have [created an environment](https://gitlab.com/mguler/lane_cluster/-/wikis/Dependency-Management#creating-conda-environments) you need to activate it before you can access its contents.
If you are running an interactive job you can do this via `conda activate ENV_NAME`. 
However, this does not always work as expected when it is part of a job submitting with `sbatch` (See [Submitting Jobs](https://gitlab.com/mguler/lane_cluster/-/wikis/Slurm-Basics#submitting-jobs) for more information on job submission).
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

## Transferring Conda Environments

Once you have set up an environment the way you like it you may want to use the same environment on a different machine.
If the other machine is running the same overarching OS as yours (running Linux locally and remotely is the most common case of this) you can simply run
```bash
conda activate ENV_NAME
conda env export | grep -v "^prefix" > environment.yaml
```
This will create an environment.yaml file that contains all the packages you install in this `conda` environment along with their frozen versions. You can then copy this file to your other system and run
```
conda create --file environment.yaml
```
to create a copy of this environment.

If the other machine runs a different operating system you can still usually transfer environments.
However, now you should use
```bash
conda activate ENV_NAME
conda env export --from-history | grep -v "^prefix" > environment.yaml
```
This will essentially serialize the exact install commands you called in this environment.
A new environment can be created in the same way as before, but now `conda` will replay the install commands instead of pulling down exact versions.

__External Resources__:
- [`conda env export` Man Page](https://veranostech.github.io/docs-korean-conda-docs/docs/build/html/commands/env/conda-env-export.html)

# Modules

[Modules](https://github.com/cea-hpc/modules) are method for modified a user's environment in a reversible way.
They can dynamically modify a user's environment.

## Lane's Default Modules
Without any other modification you can run `module avail` to see what default modules are available on Lane.
```
module avail
# output:
#
# ------------------------------------------------------- /usr/share/Modules/modulefiles --------------------------------------------------------
# cuda-10.0                          matlab-9.5                         R-3.5.3                            singularity/ImageMagick-v6.9.10-23
# cuda-8.0                           matlab-9.7                         rocks-openmpi                      singularity/KNIME-v4.1.1
# cudnn-10.0-7.3                     module-git                         rocks-openmpi_ib                   singularity/meme-suite-v5.1.0
# cudnn-8.0-7.1                      module-info                        singularity/bedtools-v2.29.2       singularity/neofetch-v7.0.0
# dot                                modules                            singularity/bftools-v6.3.1         singularity/r-base-v3.6.2
# ffmpeg-4.2.2                       null                               singularity/cellorganizer-v2.8.1   singularity/samtools-v1.10
# gurobi902                          opt-perl                           singularity/cowsay-v3.03           singularity/texlive-v3.14
# java-1.8.0                         python27                           singularity/FastQC-v0.11.9         use.own
# maple2019                          python27-extras                    singularity/gcc-v8.3.0
# mathematica-12.0                   python36                           singularity/genometools-v1.5.10
# mathematica-8.0                    python36-extras                    singularity/genometools-v1.6.0
# 
# -------------------------------------------------------------- /etc/modulefiles ---------------------------------------------------------------
```
You can see this output is split into sections based on modulefile directories.

## Modulefile Directories
The way available modules are determined is by modulefiles in directories.
You can add your own modulefile directory with `module use PATH/TO/DIR`.
Let's use my own modulefile directory and see what we have.
```bash
module use /projects/mohimanilab/mustafa/tools/modulefiles
module avail
# output (only new section)
#
# ----------------------------------------------- /projects/mohimanilab/mustafa/tools/modulefiles -----------------------------------------------
# cudatoolkit/10.0 cudnn/6.0        fingerprinter    tfcuda/1.10      tfcuda/1.15      tfcuda/1.6       tfcuda/2.1
# cudatoolkit/10.1 cudnn/7.4        proteowizard     tfcuda/1.11      tfcuda/1.2       tfcuda/1.7       tfcuda/2.2
# cudatoolkit/8.0  cudnn/7.6        rust             tfcuda/1.12      tfcuda/1.3       tfcuda/1.8       tfcuda/2.3
# cudatoolkit/9.2  db-search-tools  tfcuda/1.0       tfcuda/1.13      tfcuda/1.4       tfcuda/1.9
# cudnn/5.1        dereplicator     tfcuda/1.1       tfcuda/1.14      tfcuda/1.5       tfcuda/2.0
ls -l /projects/mohimanilab/mustafa/tools/modulefiles
# output:
#
# total 87
# drwxrwsr-x 3 mguler mohimanilab    6 Nov  4 22:59 cudatoolkit
# -rw-rw-r-- 1 mguler mohimanilab  611 Nov  4 23:07 cudatoolkit_common
# drwxrwsr-x 2 mguler mohimanilab    6 Nov  4 19:28 cudnn
# -rw-rw-r-- 1 mguler mohimanilab  625 Nov  4 23:09 cudnn_common
# -rw-rw-r-- 1 mguler mohimanilab 1667 Nov  2 11:29 db-search-tools
# -rw-rw-r-- 1 mguler mohimanilab  580 Apr  5  2020 dereplicator
# -rw-rw-r-- 1 mguler mohimanilab  368 Jun  7 15:44 fingerprinter
# -rw-rw-r-- 1 mguler mohimanilab  264 Apr 16  2020 proteowizard
# -rw-rw-r-- 1 mguler mohimanilab  276 Apr  4  2020 rust
# drwxrwsr-x 2 mguler mohimanilab   22 Nov  4 23:15 tfcuda
```
Notice that we don't see all the files in this directory when we look at `module avail`.
This is because only files that contain the [magic cookie](https://modules.readthedocs.io/en/latest/modulefile.html)
will be loaded as modules.
Let's look at a few files.
```bash
head -n 1 /projects/mohimanilab/mustafa/tools/modulefiles/rust
# output:
#
# #%Module
head -n 1 /projects/mohimanilab/mustafa/tools/modulefiles/cudatoolkit/10.0
# output:
#
# #%Module
head -n 1
# output:
#
# # Common modulefile for cuDNN
```
Only the files that begin with `#%Module` are counted as modulefiles for `module avail`.

## Modulefile Format
We showed previously modulefiles must begin with the magic cookie `#%Module`, but what do modulefiles consist of?
Modulefiles are [Tcl](https://github.com/tcltk/tcl) scripts that modify a user's environment.
Let's look at my `rust` module as an example.
```bash
# contents of /projects/mohimanilab/mustafa/tools/modulefiles/rust
#%Module
##
## Rust modulefile
##
## modulefiles/
##

set ver 1.39.0
set msg "This module adds necessary paths and variables to use rust"

proc ModulesHelp { } {
    puts stderr $msg
}

module-whatis "Use rust $ver"

prepend-path PATH /projects/mohimanilab/mustafa/cargo/bin
```
This is a very simple modulefile. It sets the variable `ver` to `1.39.0`, adds a `whatis` definition and then simply prepends the path to my rust installation to `PATH`.
You can get a summary of what a module does with the `module show` command.
```bash
module show rust
# output:
#
# ------------------------------------------------------------------
# /projects/mohimanilab/mustafa/tools/modulefiles/rust:
# 
# module-whatis    Use rust 1.39.0
# prepend-path     PATH /projects/mohimanilab/mustafa/cargo/bin
# -------------------------------------------------------------------
module show db-search-tools
# output:
#
# -------------------------------------------------------------------
# /projects/mohimanilab/mustafa/tools/modulefiles/db-search-tools:
# 
# module           load rust
# module-whatis    Use MAGMa+, CFM-ID, and CSI:FingerID
# prepend-path     PATH /projects/mohimanilab/mustafa/tools/time-1.9/bin
# setenv           MAGMAPLUS_CLASSIFIER_PATH /projects/mohimanilab/mustafa/tools/magma+/MAGMa-plus
# setenv           MAGMA_ROOT /projects/mohimanilab/mustafa/tools/magma+/MAGMa-plus
# setenv           JAVA_HOME /projects/mohimanilab/mustafa/tools/java/jdk-11.0.7+10-jre/
# prepend-path     PATH /projects/mohimanilab/mustafa/tools/magma+/bin
# prepend-path     LD_LIBRARY_PATH /projects/mohimanilab/mustafa/tools/cfm-id/boost/boost_1_59_0
# prepend-path     LD_LIBRARY_PATH /projects/mohimanilab/mustafa/tools/cfm-id/rdkit/RDKit_2013_09_1/lib
# prepend-path     LD_LIBRARY_PATH /projects/mohimanilab/mustafa/tools/cfm-id/lp_solve/lp_solve_5.5/lpsolve55/bin/ux64
# prepend-path     LD_LIBRARY_PATH /projects/mohimanilab/mustafa/tools/cfm-id/liblbfgs/lib
# prepend-path     PATH /projects/mohimanilab/mustafa/tools/cfm-id/cfm/bin
# prepend-path     PATH /projects/mohimanilab/mustafa/tools/java/jdk-11.0.7+10-jre/bin
# prepend-path     PATH /projects/mohimanilab/mustafa/tools/csi-fingerid/sirius-linux64-headless-4.4.29/bin
# prepend-path     PATH /projects/mohimanilab/mustafa/dereplicator/cycloquest_minimal
# setenv           DEREP_BASE /projects/mohimanilab/mustafa/dereplicator/cycloquest_minimal
# prepend-path     PATH /projects/mohimanilab/mustafa/tools/search_runner/target/release
# -------------------------------------------------------------------
```
The most commonly used commands are `setenv` which sets an environment variable that is active when the module is loaded and `prepend-path` which adds a directory to a `PATH`-like variable when the module is loaded.

## Using Modules
Once you have added a modulefile directory with `module use` you can load modules with `module load` and unload them with `module unload`.
At any point you can use `module list` to see what modules you have active.
Additionally, you can use `module purge` to unload all loaded modules.
You can add `module load` commands to the beginning of jobs scripts to use modules in your jobs.
For more information on submitting jobs see [Submitting Jobs](https://gitlab.com/mguler/lane_cluster/-/wikis/Slurm-Basics#submitting-jobs).

## Module Command Summary

| Command | Description |
| :------ | :---------- |
| `module load MODULE` | Activates a module `MODULE`, running its modulefile |
| `module unload MODULE` | Removes an active module `MODULE`, undoing everything done in its modulefile |
| `module add MODULE` | Same as `module load MODULE` |  
| `module rm MODULE` | Same as `module unload MODULE` |  
| `module show MODULE` | Show what module `MODULE` will do when loaded |  
| `module list` | Show all loaded modules | 
| `module avail` | Show all available modules | 
| `module purge` | Unload all loaded modules | 
| `module use DIR` | Use `DIR` as a modulefile directory |
