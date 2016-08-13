---
layout: post
title: "mount your usb into your writing system"
date: 2016-07-21 16:47:24 +0800
comments: true
categories: life
---

1. fdisk -l
2. mount -o loop /dev/sdb4 /mnt

<!--more-->

1. fdisk -l  to find your usb media (FAT32 system)
```
Device     Boot      Start        End   Sectors   Size Id Type
/dev/sda1  *         16072  401024119 401008048 191.2G  7 HPFS/NTFS/exFAT
/dev/sda2        401025024  638497680 237472657 113.2G 83 Linux
/dev/sda3        638498816 1259280383 620781568   296G  7 HPFS/NTFS/exFAT
/dev/sda4       1259282430 1465145343 205862914  98.2G  f W95 Ext'd (LBA)
/dev/sda5       1259282432 1465145343 205862912  98.2G  7 HPFS/NTFS/exFAT




Disk /dev/sdb: 14.9 GiB, 15998099456 bytes, 31246288 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xcad4ebea

Device     Boot Start      End  Sectors  Size Id Type
/dev/sdb4  *      256 31246287 31246032 14.9G  c W95 FAT32 (LBA)
```

2. then mount it to the folder ,such as /mnt

```
    mount -o loop /dev/sdb4 /mnt
```


That's all.

Noted.
