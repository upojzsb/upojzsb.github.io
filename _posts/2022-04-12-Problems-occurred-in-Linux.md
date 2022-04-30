---
layout:     post
title:      Problems occurred in Linux
subtitle:   Linux
date:       2022-04-12
author:     UPOJZSB
header-img: img/post-bg-random.jpg
catalog: true
tags:
    - Problem
    - Linux
    - FUSE
---

# Prologue

> Sorted by alphabetical order of the name of the packages

# FUSE
## df: ‘/home/shendi_zl/iso/mnt’: Transport endpoint is not connected

Incorrect operation will cause some problems to the mount point mounted by FUSE, and something like `Transport endpoint is not connected` will occur.

## Solution

No need for root privilege.

```bash
fusermount -uz mount_point
```

*Updated at 12 Apr, 2022*

## Reference
[Reference](https://toolspond.com/fix-transport-endpoint-is-not-connected/)
