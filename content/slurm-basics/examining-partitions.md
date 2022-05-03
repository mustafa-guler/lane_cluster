---
title: "Examining Partitions"
weight: 40
---
When logging into a new Slurm cluster you can examine characteristics of available partitions with the [`sinfo`](https://slurm.schedmd.com/sinfo.html) command.
Below are some useful examples for Lane.
Your available partition lists may look different.

```bash
# List names of partitions you have access to
sinfo -o "%20P"
# output:
#
# PARTITION
# DEBUG
# short1
# interactive
# moh1
# moh2-gpu
# pool1
# pool3-bigmem

# List total resources for each partition
# CPUS and MEMORY are given per node
# NODES are given in the format Allocated/Idle
sinfo -o "%20P %10A %10c %10m %25f %20G %20N"
# output:
#
# PARTITION            NODES(A/I) CPUS       MEMORY     AVAIL_FEATURES            GRES                 NODELIST
# DEBUG                0/0        0          0          (null)                    (null)
# short1               0/3        8          32000+     rack-0,8CPUs              (null)               compute-0-[9-11]
# short1               1/2        32         64153      rack-3,32CPUs             (null)               compute-3-[32-34]
# interactive          0/3        8          32000+     rack-0,8CPUs              (null)               compute-0-[9-11]
# moh1                 2/3        48         257364     rack-0,48CPUs             (null)               compute-0-[7,20-21,2
# moh1                 0/2        56         128530     rack-1,56CPUs             (null)               compute-1-[2-6]
# moh2-gpu             0/1        32         257348     rack-4,32CPUs,            gpu:4                compute-4-18
# pool1                0/4        16         48059      rack-0,16CPUs             (null)               compute-0-[4-6,8]
# pool1                0/8        8          23936      rack-0,8CPUs              (null)               compute-0-[22-28,30]
# pool1                0/11       8          23933+     rack-1,8CPUs              (null)               compute-1-[7-10,20-2
# pool1                0/5        16         48059      rack-1,16CPUs             (null)               compute-1-[14-18]
# pool3-bigmem         0/1        32         257757     rack-0,32CPUs             (null)               compute-0-12
# pool3-bigmem         0/1        8          128761     rack-1,8CPUs              (null)               compute-1-35

# List free resources 
# CPUS in the format Allocated/Idle/Other/Total
sinfo -o "%20P %20e %20m %20C %f"
# output:
#
# PARTITION            FREE_MEM             MEMORY               CPUS(A/I/O/T)        AVAIL_FEATURES
# DEBUG                0                    0                    0/0/0/0              (null)
# short1               29863-45543          32000+               0/24/0/24            rack-0,8CPUs
# short1               39072-61998          64153                16/80/0/96           rack-3,32CPUs
# interactive          29863-45543          32000+               0/24/0/24            rack-0,8CPUs
# moh1                 29816-250476         257364               148/92/240/480       rack-0,48CPUs
# moh1                 34069-94091          128530               0/168/112/280        rack-1,56CPUs
# moh2-gpu             167177               257348               1/31/0/32            rack-4,32CPUs,
# pool1                13063-N/A            23933+               2/86/8/96            rack-1,8CPUs
# pool1                40995-45646          48059                0/64/0/64            rack-0,16CPUs
# pool1                21716-23147          23936                0/64/0/64            rack-0,8CPUs
# pool1                30473-45375          48059                3/77/0/80            rack-1,16CPUs
# pool3-bigmem         37394                128761               1/7/0/8              rack-1,8CPUs
# pool3-bigmem         254574               257757               0/32/0/32            rack-0,32CPUs
```

## Shared Partitions on Lane
| Partition Name | Description |
| :------------- | :---------- |
| short1 | I've never used this. It seems to be intended for short jobs |
| interactive | Intended for interactive sessions. See [Interactive Sessions](https://gitlab.com/mguler/lane_cluster/-/wikis/Slurm-Basics#interactive-sessions) |
| pool1 | This is the shared partition across all of CBD. |
| pool3-bigmem | This is a large memory partition shared across all of CBD | 

Your may have access to more partitions specific to your group. If so, please ask your advisor or a senior student the purpose of those partitions.


## External Resources
- [`sinfo` Man Page](https://slurm.schedmd.com/sinfo.html)
