---
title: "Interactive Sessions"
weight: 20
---
The way clusters are generally designed is there is a single, relatively small computer used as a "head" node.
Users when executing `ssh` are actually connecting to this head node.
Since this node handles all the initial user connection traffic nothing computationally intensive should be run by users on the head node.
Processes that use large amounts of memory, CPU, or execute a lot of IO commands on the head node slow logins for other users severely.

The easiest way to avoid accidentally doing this is to immediately request an interactive session after connecting to a cluster.
On Lane you can run the following.
```
srun -n 1 -p interactive --pty bash
```
If it worked you will now see `@compute-...` in your shell prompt. You can now run anything you like, it will not affect other users.
To release your interactive session and return to the head node run `exit` or type ctrl-d. 
