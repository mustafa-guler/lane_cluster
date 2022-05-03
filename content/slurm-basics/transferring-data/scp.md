---
title: "scp"
weight: 0
---
The simplest way to transfer files is via `scp`. This functions similarly to `cp`, but operates over a network connection.
It will respect your `ssh` configuration, so you can use `Host` definitions in the commands.
For example, if you have set up the Lane `ssh` configuration entry as suggested you can do the following to download a file to your current local directory:
```bash
scp lane:/PATH/TO/FILE .
```
Uploading operates similarly.
```bash
scp /PATH/TO/FILE lane:/PATH/TO/REMOTE/DESTINATION
```
For directories use the `-r` flag to download/upload recursively.
If possible, it's suggested to compress files before transfer.
To avoid relative path resolution, which could introduce errors, you can use `readlink` to get an absolute file path.
For example:
```bash
scp $(readlink -f ./RELATIVE/PATH) lane:/PATH/TO/REMOTE/DESTINATION
```

For Windows machines see [WinSCP](https://winscp.net/eng/index.php).

## External Resources
- [`scp` Man Page](https://linux.die.net/man/1/scp)
- [WinSCP Site](https://winscp.net/eng/index.php)
