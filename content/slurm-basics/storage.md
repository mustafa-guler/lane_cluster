---
title: "Storage on Lane"
weight: 50
---
There are a few storage systems on Lane.
First is your home directory (`~`, `/home/$(whoami)`).
Home directories have `500GB` are by default are only accessible by you.
You can examine characteristics of your home directory with the command
```bash
df -ah | grep -P "(Filesystem|$(whoami))"
# output:
#
# Filesystem                                         Size  Used Avail Use% Mounted on
# nas-3-31:/volume01/home/mguler                     500G   54G  447G  11% /home/mguler
```

In addition to your home directory you will likely have access to network attached storage (NAS) filesystems specific to your lab. Please consult with your advisor or senior students for questions about those filesystems.

## External Resources
- [`df` Man Page](https://www.man7.org/linux/man-pages/man1/df.1.html)
