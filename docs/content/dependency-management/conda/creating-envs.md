---
title: "Creating Conda Environments"
weight: 10
---
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
| `--file` | Path to environment.yaml file described in [Transferring Conda Environments](/dependency-management/conda/transferring-envs) |
| `--clone` | Name of `conda` environment to copy for this new environment |

## External Resources
- [`conda-forge`](https://conda-forge.org/)
- [`bioconda`](https://bioconda.github.io/)
- [anaconda package repository](https://anaconda.org/anaconda/repo)
