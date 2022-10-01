---
layout:     post
title:      Clone a Linux System via dd
subtitle:   Linux
date:       2022-09-30
author:     UPOJZSB
header-img: img/post-bg-random.jpg
catalog: true
tags:
    - OS
    - Linux
    - File-System
---

# Prologue

Since one of the SSD of my BB's laptop is used in a terrible circumstance, its wear level percentage comes near to its designed lifetime. We made an decision that we should move the system and data to another Samsung SSD to prevent data loss.

The operations below is been done under a *Live USB*, see [Create a bootable USB stick with Rufus on Windows
](https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#1-overview) for detail.

All operations performed below is been don via root privilege.
# Backup Data

**DO NOT** forget to backup your data before any operations related to a disk.

# Data Migration

Because the size of the source SSD is equivalent to the target one, we can simply use `dd` to perform an end-to-end data migration:

```bash
dd if=/dev/nvme0n1 of=/dev/nvme1n1
```

And we can see the progress by sending USR1 signal to `dd`.

```bash
watch -d kill -USR1 12345
# 12345 is the PID of dd
```

# Change the UUID of Partition

Since we use `dd` to implement data migration, the UUID of partition of target disk is equal to the source one, and it may affect boot progress because we marked them in `/etc/fstab` by their UUID.

Therefore, we should modify the UUID of the partition of the target disk to prevent the ambiguity. Perform following command for each target partitions.

```bash
umount /dev/nvme1n1p1 # Make sure that it's idle
tune2fs -U random /dev/nvme1n1p1  # Change the UUID randomly
# nvme1n1p1 is the device name of the partition, perform commands above for each partitions
```

# Rebuild Boot

## In Live USB Environment

After operations above, we have already migrated all system and data we need, but the system is still not bootable, so we have to reconstruction boot progress.

Firstly, we `mount` the partitions of the target disk to the environment we are using:
```bash
mount /dev/nvme1n1p1 /mnt
mount /dev/nvme1n1p2 /mnt/boot
mount /dev/nvme1n1p3 /mnt/home
# Mount the target disk partition according to your real directory construction
mount --bind /proc /mnt/proc
mount --bind /sys /mnt/sys
mount --bind /dev /mnt/dev
# Mount the virtual filesystem by binding them with the host
```

Because we have to use `chroot` to do something to the `/dev` devices, we should mount `/dev` filesystem by `--bind` option.

## In CHROOT Environment

After `mount` the filesystems correctly, we can `chroot` to our target environment:

```bash
chroot /mnt
```

### Change fstab

Because we have changed the UUID of partitions, the `/etc/fstab` file should be modified accordingly. Here, we can refer to `/def/mtab`, which is the currrent `mount` status.

```bash
cat /etc/mtab > /etc/fstab
```

We highly recommend to use UUID rather than device name in `/etc/fstab` file, because the device name may vary. We can see the UUID of the partitions by `blkid`.

```bash
blkid
```

And change `/etc/fstab` file to use UUID rather than device name.

```bash
vim /etc/fstab
# The format of /etc/fstab is like this
## <device>                                <dir> <type> <options> <dump> <fsck>
# UUID=0a3407de-014b-458b-b5c1-848e92a327a3 /     ext4   noatime   0      1
# UUID=f9fe0b69-a280-415d-a03a-a32752370dee none  swap   defaults  0      0
# UUID=b411dc99-f0a0-4c87-9e05-184977be8539 /home ext4   noatime   0      2
```

### Deal with GRUB

Finally, we can reconstruct the `gurb`, an opensource bootloader.

We first install it on our new disk:
```bash
grub-install /dev/nvme1n1
```

Then, we remove the configuration file of the previous disk, because the new generation progress may be affected by it.

```bash
rm -f /boot/grub/grub.cfg
```

And finally, we re-generate a configuration file of `grub`:
```bash
grub-mkconfig -o /boot/grub/grub.cfg
```

After that, we can quit the `chroot` environment, reboot and enjoy our new disk :-).

# Alternative

Someone says we can use `boot-repair` instead, but I failed several times.

# References

[How do you monitor the progress of dd?](https://askubuntu.com/questions/215505/how-do-you-monitor-the-progress-of-dd)

[How to Change UUID of Partition in Linux Filesystem](https://www.tecmint.com/change-uuid-of-partition-in-linux/)

[mount dev, proc, sys in a chroot environment?](https://superuser.com/questions/165116/mount-dev-proc-sys-in-a-chroot-environment)

[Boot-Repair](https://help.ubuntu.com/community/Boot-Repair)
