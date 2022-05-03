---
title: "Installing Conda"
weight: 0
---
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

## External Resources
- [Miniconda Installation Page](https://docs.conda.io/en/latest/miniconda.html)
