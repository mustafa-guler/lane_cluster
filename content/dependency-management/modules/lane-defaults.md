---
title: "Lane's Default Modules"
weight: 0
---
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
