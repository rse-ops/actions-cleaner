# Actions Cleaner

This is an action to try to make as much space on the GitHub runner as we can!
Yes, we are building LLVM. 😂️

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
