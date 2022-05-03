---
title: "rsync"
weight: 10
---
This is another `cp`-like command with support for file transfer over networks.
However, as the name suggests, this is a sync operation.
This means that if some files exist unmodified in both the source and destination they will not be copied, saving some time.
Additionally, `rsync` is considerably more flexible than `scp` with the trade-off of being a bit more complicated to use.
Just like `scp`, `rsync` respects `ssh` configurations.
The man page for `rsync` includes many examples that are useful.


## External Resources
- [`rsync` Man Page](https://linux.die.net/man/1/rsync)
- [Nice Summary of Differences between `rsync` and `scp`](https://stackoverflow.com/questions/20244585/how-does-scp-differ-from-rsync)
