---
title: "Connecting to Lane"
weight: 10
---

The Lane cluster has the host name `lanec1.compbio.cs.cmu.edu`.
You can connect to Lane via `ssh` with the command
```
ssh USERNAME@lanec1.compbio.cs.cmu.edu
```
for Linux and OSX systems. If you are using Windows you can either use [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/) or [WSL](https://docs.microsoft.com/en-us/windows/wsl/about). All following instructions operate under the assumption you use `WSL` if you're a Windows user.

For a more compact representation you can create directory `~/.ssh` if it doesn't exist and store login information for Lane as follows.
```Shell
mkdir -p ~/.ssh
cd ~/.ssh
cat << EOL >> config
Host lane
    User USERNAME
    HostName lanec1.compbio.cs.cmu.edu
EOL
```
The above configuration creates a local configuration entry for a remote host `lanec1.compbio.cs.cmu.edu` for a user with remote username `USERNAME`.
This whole configuration is stored under the name `lane`.
Now, to `ssh` into lane all you need to run is `ssh lane`.
This `ssh` configuration file also works with `scp` and `rsync`.
You can also add your password here. I strongly recommend you don't.

## External Resources
- [`ssh` config information](https://www.ssh.com/ssh/config/)
- [`scp` man page](https://linux.die.net/man/1/scp)
- [`rsync` man page](https://linux.die.net/man/1/rsync)
