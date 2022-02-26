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
else wants to add others! If you need space in addition to this, you can try out
[easimon/maximize-build-space](https://github.com/easimon/maximize-build-space#how-it-works) which
tries to consolidate filesystems.


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
