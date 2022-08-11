# Actions Cleaner

We love GitHub and want to use it for all-the-things, but sometimes the runner
runs out of space with our chonker builds! This is an action to try to make as 
much space on the GitHub runner as we can! Yes, we are sometimes building LLVM. 😂️

```yaml
name: Actions Cleaner

on: 
  pull_request: []
 
jobs:
  update:
    name: Clean All The Things!
    runs-on: ubuntu-latest
    steps:
      - name: Run Actions Cleaner
        uses: rse-ops/actions-cleaner/ubuntu@main
```

For detailed usage, see the actions.yml in [ubuntu](ubuntu) - mostly every removal can be customized so you
can instead keep it! Here is an example:


```yaml
name: Actions Cleaner

on: 
  pull_request: []
 
jobs:
  update:
    name: Clean All The Things!
    runs-on: ubuntu-latest
    steps:
      - name: Run Actions Cleaner
        uses: rse-ops/actions-cleaner/ubuntu@main
        with:
          remove_node: false
          remove_ruby: false
```

We have them organized by base runner in case someone
else wants to add others! 

## Frequently Asked Questions

> Can I run this on a container?

Yes, and you'll need to set `docker_purge` to false since Docker does not exist inside Docker!
The remaining commands should support using sudo/or not.

> Why does it take 10 minutes?

There is a tradeoff between uninstalling stuff and time. If you uninstall nothing, your builds will start right away, but you get no extra free space.
If you uninstall everything, you have to wait and get the maximum space. If you use this action we recommend tweaking for your use case! The default
is to set true for everything, the assumption being that most people will be more selective about adding things back in.

> Why do you apt uninstall patterns like `linux-azure-*`?

The assumption here is that if someone wants to uninstall something in a namespace,
they want to uninstall everything in the namespace. By using patterns are recipe is less likely
to get outdated over time (and thus will have less maintanance work).

> Why do you catch a lot of apt remove errors?

You mean with a pipe to an echo? We do this for the same reason as the previous bullet!
If a runner changes slightly we don't want everyone's workflows to fail because of one thing.
That said, if you see a change or an issue, please [let us know](https://github.com/rse-ops/actions-cleaner/issues).

> Can we add other runners or things to remove?

Yes of course! If you want to open a pull request, we welcome contributions. Or if you want
to request a small tweak or have a question, please [open an issue](https://github.com/rse-ops/actions-cleaner/issues).

> Have you thought about messing around with swap / mounts?

There is a sibling action [easimon/maximize-build-space](https://github.com/easimon/maximize-build-space#how-it-works) that
we recommend trying for this. You should run it before this one.

> Where can I learn more about GitHub runners?

[Here you go!](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners)
We ❤️ you GitHub! 

## Metrics

GitHub runners change over time so this reflects the point in time when we first
created the action. With everything flagged to remove as true (default) we start with these stats:

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/root        84G   51G   33G  61% /
devtmpfs        3.4G     0  3.4G   0% /dev
tmpfs           3.4G  4.0K  3.4G   1% /dev/shm
tmpfs           695M  1.1M  694M   1% /run
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           3.4G     0  3.4G   0% /sys/fs/cgroup
/dev/loop0       68M   68M     0 100% /snap/lxd/21835
/dev/sda15      105M  5.2M  100M   5% /boot/efi
/dev/loop2       44M   44M     0 100% /snap/snapd/14549
/dev/loop1       62M   62M     0 100% /snap/core20/1328
/dev/sdb1        14G  4.1G  9.0G  32% /mnt
```
And end up with:

```bash
Filesystem      Size  Used Avail Use% Mounted on
/dev/root        84G   21G   63G  25% /
devtmpfs        3.4G     0  3.4G   0% /dev
tmpfs           3.4G  4.0K  3.4G   1% /dev/shm
tmpfs           695M  1.1M  694M   1% /run
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           3.4G     0  3.4G   0% /sys/fs/cgroup
/dev/sda15      105M  5.2M  100M   5% /boot/efi
/dev/sdb1        14G  4.1G  9.0G  32% /mnt
```

So we now have 63G free space at `/`, up 30G from 33G. That's pretty good!
Happy building!

License
-------

Copyright (c) 2017-2021, Lawrence Livermore National Security, LLC. 
Produced at the Lawrence Livermore National Laboratory.

RADIUSS Docker is licensed under the MIT license [LICENSE](./LICENSE).

Copyrights and patents in the RADIUSS Docker project are retained by
contributors. No copyright assignment is required to contribute to RADIUSS
Docker.

This work was produced under the auspices of the U.S. Department of
Energy by Lawrence Livermore National Laboratory under Contract
DE-AC52-07NA27344.
