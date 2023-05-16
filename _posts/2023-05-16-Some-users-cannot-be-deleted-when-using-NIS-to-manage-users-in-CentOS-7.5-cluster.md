---
layout:     post
title:      Some users cannot be deleted when using NIS to manage users in CentOS 7.5 cluster
subtitle:   Problem
date:       2023-05-16
author:     UPOJZSB
header-img: img/Linux.png
catalog: true
tags:
    - Linux
    - CentOS
    - Problem
    - NIS
    - User
---

# Problem

The cluster master and computing nodes are using CentOS Linux release 7.5.1804 (Core). File ``/etc/nsswitch.conf` contains

```
passwd: files nis sss
shadow: files nis sss
group: files nis sss
```

The output of `getent passwd -s nis` includes

```
u2:$1$GE2edQgf$UyXuGspuf5uwbp.zK91aH0:9002:9002::/data/home/geou2:/bin/csh
u3:$1$Ddytc7s0$5yPBahgmfVyaqC0.Bur1.1:9003:9003::/data/home/geou3:/bin/csh
u1:$1$3Z52ZIDu$Od.rfOebsXRxI.nrBut1G1:9001:9001::/data/home/geou1:/bin/csh
```
The output of `ypcat passwd` also includes

```
u2:$1$GE2edQgf$UyXuGspuf5uwbp.zK91aH0:9002:9002::/data/home/geou2:/bin/csh
u3:$1$Ddytc7s0$5yPBahgmfVyaqC0.Bur1.1:9003:9003::/data/home/geou3:/bin/csh
u1:$1$3Z52ZIDu$Od.rfOebsXRxI.nrBut1G1:9001:9001::/data/home/geou1:/bin/csh
```

I can use su to switch into these users:

```bash
$su u1
Password.
mkdir: cannot create directory '/data': Permission denied
Attempting to create directory /data/home/u1/perl5
mkdir /data: Permission denied at /usr/share/perl5/vendor_perl/local/lib.pm line 269.
BEGIN failed--compilation aborted.
[u1@mu01 /]$
```

But deleting and creating a user with the same name results in the following error:
```bash
# userdel u1
userdel: Unable to remove u1 from /etc/passwd
# useradd u1
useradd: user u1 already exists
```
There are no such users in `/etc/{passwd, shadow}`. (There are some corresponding groups where this is also the case)

The computing nodes can't log in to the above users:

```bash
[user@cu01 ~]$ su u1
su: user u1 does not exist
```

Executing:

```bash
makedbm -u passwd.byname
makedbm -u passwd.byuid
makedbm -u group.byname
makedbm -u group.byuid
```

None of the above users (groups) appear.

Since I need to install some commercial software, the user name and group name must be preset.

How can I delete these users and groups? Thanks for your help!

Tried `cd /var/yp/; make`, no luck.

# Solution

I tried using `strace -o stypcat ypcat passwd` to figure out where `ypcat` was getting those usernames from and found that there were some `socket` and `connect` system calls pointing to another IP.

I logged into that IP (representing the management node of the other cluster), and those users appeared in `/etc/passwd` on that machine. I also found that the results for `domainname` were the same for both clusters.

It seems that the NIS of our cluster's management node treats another cluster's management node as its parent node.

Finally, I removed the `nis` method from the `nsswitch.conf` on the master node, and remain it on the child nodes. So the `nsswitch.conf` of the master node looks like:

```
passwd: files sss
shadow: files sss
group: files sss
```

Since the users of the master node are listed on the `/etc/passwd`, the `NIS` server hasn't been stopped and the child nodes' `nsswitch.conf` files remain unchanged, we can still login child nodes from the master node via NIS service.

# Reference

Linux Manuals

*I asked this question on [StackExchange](https://unix.stackexchange.com/questions/745974/some-users-groups-cannot-be-deleted-when-using-nis-to-manage-users-in-centos-7) and [V2EX](https://v2ex.com/t/940263).*
