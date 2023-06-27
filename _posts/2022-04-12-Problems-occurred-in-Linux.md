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
    - SSH
    - SSHD
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

# SSH and SSHD
## Users cannot login Linux host with pubkeys via ssh but root user can do so

When login a Linux host via `ssh`, some hosts requests your password even if the public key is added in `~/.ssh/authoirzed_keys` correctly, and the permission of the corresponding directory and files are set properly.

By checking the `/var/log/{secure,audit/audit.log}`, we found out that SELinux denied the key. And our normal hosts disabled the SELinux, so we decided to disable the SELinux by:

```bash
setenforce 0
```

To make the setting permanent, we edited the `/etc/selinux/config` as following:

```config
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=disabled
# SELINUXTYPE= can take one of three values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected.
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
```

The setting will be enforced after rebooting.

### Reference

[public key authentication fails ONLY when sshd is daemon](https://serverfault.com/questions/321534/public-key-authentication-fails-only-when-sshd-is-daemon)

[Why am I still getting a password prompt with ssh with public key authentication?](https://unix.stackexchange.com/questions/36540/why-am-i-still-getting-a-password-prompt-with-ssh-with-public-key-authentication)

[How to Disable SELinux on CentOS 7](https://linuxize.com/post/how-to-disable-selinux-on-centos-7/)

*Update at 27 June, 2023*


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
Linux manuals.

*Update at 15 May, 2023*
