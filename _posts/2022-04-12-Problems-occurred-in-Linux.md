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
    - Nautilus
    - User
    - Group
    - NIS
    - SSS
---

# Prologue

> Sorted by alphabetical order of the name of the packages

# FUSE
## df: ‘/home/shendi_zl/iso/mnt’: Transport endpoint is not connected

Incorrect operation will cause some problems to the mount point mounted by FUSE, and something like `Transport endpoint is not connected` will occur.

### Solution

No need for root privilege.

```bash
fusermount -uz mount_point
```

### Reference
[Reference](https://toolspond.com/fix-transport-endpoint-is-not-connected/)

*Updated at 12 Apr, 2022*

# Nautilus
## Nautilus is slow when opening directories

In Ubuntu, too many Template files can cause nautilus to slow down. Keep files in the `~/Templates` directory only if necessary.

### Reference
[Reference](https://www.reddit.com/r/pop_os/comments/rvvksq/comment/hr8cf5p/?utm_source=share&utm_medium=web2x&context=3)

*Update at 7 Oct, 2022*

# Users & Groups
## Users/Groups exist (which can be switch in by `su`) but cannot be found in `/etc/{passwd, shadow, group}`

Try to figure out the content in `/etc/nsswitch.conf`.

For example, if the following contents are in `nsswitch.conf`:
```conf
passwd: files nis sss
shadow: files nis sss
group:  files nis sss
```
It means that user and group information can be stored in three places: files (`/etc/{passwd, shadow, group}`), nis ([Network Information Service](https://wiki.archlinux.org/title/NIS)) and sss ([System Security Services](https://sssd.io/)). You can find exact which database the information in by using `getent passwd -s {files, nis, sss}`, respectively(The curly brackets "**{}**" means select one from the set in it).

### Reference
Linux manual.



*Update at 15 May, 2023*
