---
title: "Modulefile Directories"
weight: 10
---
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
