---
title: "Transferring Conda Environments"
weight: 30
---
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

## External Resources
- [`conda env export` Man Page](https://veranostech.github.io/docs-korean-conda-docs/docs/build/html/commands/env/conda-env-export.html)

