---
title: "Defining Terms"
draft: false
weight: 0
---

There are some terms I use throughout that should be explained first to remove ambiguity.

| Term | Definition |
| :--- | :--------- |
| cluster | A remote server that consists of a collection of computers |
| resources | CPUs, memory, GPUs, etc. that can be consumed by a program |
| node | A single computer in a cluster, contains resources |
| partition | A collection of nodes in a cluster with shared characteristics | 
| job | Some executable submitted to the cluster to run on some partition with some resources. Usually submitted via `sbatch` |
| task | An atomic logical unit of a job. A job can have more than one task that runs within its allocated resources. Usually started via `srun` |
