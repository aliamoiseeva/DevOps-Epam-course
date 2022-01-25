# Homework

### 1. Imagine you was asked to add new partition to your host for backup purposes. To simulate appearance of new physical disk in your server, please create new disk in Virtual Box (5 GB) and attach it to your virtual machine.
### Also imagine your system started experiencing RAM leak in one of the applications, thus while developers try to debug and fix it, you need to mitigate OutOfMemory errors; you will do it by adding some swap space.

```bash
[alia2@localhost ~]$ lsblk -o NAME,HCTL,SIZE,MOUNTPOINT | grep -i "sd"
sda             2:0:0:0    18.3G
├─sda1                        1G /boot
└─sda2                     17.2G
sdb             3:0:0:0       5G
```

#### /dev/sdc - 5GB disk, that you just attached to the VM (in your case it may appear as /dev/sdb, /dev/sdc or other, it doesn't matter)
#### 1.1. Create a 2GB   !!! GPT !!!   partition on /dev/sdc of type "Linux filesystem" (means all the following partitions created in the following steps on /dev/sdc will be GPT as well)

```bash
[alia2@localhost ~]$ sudo fdisk /dev/sdb -l
[sudo] password for alia2:

Disk /dev/sdb: 5368 MB, 5368709120 bytes, 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes

[alia2@localhost ~]$ sudo gdisk /dev/sdb
GPT fdisk (gdisk) version 0.8.10

Partition table scan:
  MBR: not present
  BSD: not present
  APM: not present
  GPT: not present

Creating new GPT entries.

Command (? for help): n
Partition number (1-128, default 1):
First sector (34-10485726, default = 2048) or {+-}size{KMGTP}:
Last sector (2048-10485726, default = 10485726) or {+-}size{KMGTP}: +2G
Current type is 'Linux filesystem'
Hex code or GUID (L to show codes, Enter = 8300):
Changed type of partition to 'Linux filesystem'

Command (? for help): w

Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
PARTITIONS!!

Do you want to proceed? (Y/N): y
OK; writing new GUID partition table (GPT) to /dev/sdb.
The operation has completed successfully.

[alia2@localhost ~]$ sudo gdisk /dev/sdb
GPT fdisk (gdisk) version 0.8.10

Partition table scan:
  MBR: protective
  BSD: not present
  APM: not present
  GPT: present

Found valid GPT with protective MBR; using GPT.

Command (? for help): p
Disk /dev/sdb: 10485760 sectors, 5.0 GiB
Logical sector size: 512 bytes
Disk identifier (GUID): E6C6E131-F02D-4868-B3C4-26A6FB0B9188
Partition table holds up to 128 entries
First usable sector is 34, last usable sector is 10485726
Partitions will be aligned on 2048-sector boundaries
Total free space is 6291389 sectors (3.0 GiB)

Number  Start (sector)    End (sector)  Size       Code  Name
   1            2048         4196351   2.0 GiB     8300  Linux filesystem

```

#### 1.2. Create a 512MB partition on /dev/sdc of type "Linux swap"

```bash
[alia2@localhost ~]$ sudo gdisk /dev/sdb
GPT fdisk (gdisk) version 0.8.10

Partition table scan:
  MBR: protective
  BSD: not present
  APM: not present
  GPT: present

Found valid GPT with protective MBR; using GPT.

Command (? for help): n
Partition number (2-128, default 2):
First sector (34-10485726, default = 4196352) or {+-}size{KMGTP}:
Last sector (4196352-10485726, default = 10485726) or {+-}size{KMGTP}: +512M
Current type is 'Linux filesystem'
Hex code or GUID (L to show codes, Enter = 8300): 8200
Changed type of partition to 'Linux swap'

Command (? for help): w

Final checks complete. About to write GPT data. THIS WILL OVERWRITE EXISTING
PARTITIONS!!

Do you want to proceed? (Y/N): y
OK; writing new GUID partition table (GPT) to /dev/sdb.
The operation has completed successfully.
[alia2@localhost ~]$ sudo gdisk /dev/sdb
GPT fdisk (gdisk) version 0.8.10

Partition table scan:
  MBR: protective
  BSD: not present
  APM: not present
  GPT: present

Found valid GPT with protective MBR; using GPT.

Command (? for help): p
Disk /dev/sdb: 10485760 sectors, 5.0 GiB
Logical sector size: 512 bytes
Disk identifier (GUID): E6C6E131-F02D-4868-B3C4-26A6FB0B9188
Partition table holds up to 128 entries
First usable sector is 34, last usable sector is 10485726
Partitions will be aligned on 2048-sector boundaries
Total free space is 5242813 sectors (2.5 GiB)

Number  Start (sector)    End (sector)  Size       Code  Name
   1            2048         4196351   2.0 GiB     8300  Linux filesystem
   2         4196352         5244927   512.0 MiB   8200  Linux swap
```

#### 1.3. Format the 2GB partition with an XFS file system

```bash
[alia2@localhost ~]$ sudo mkfs.xfs -f -L XFS /dev/sdb1
meta-data=/dev/sdb1              isize=512    agcount=4, agsize=131072 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=524288, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
```

#### 1.4. Initialize 512MB partition as swap space

```bash
[alia2@localhost ~]$ sudo mkswap /dev/sdb2
Setting up swapspace version 1, size = 524284 KiB
no label, UUID=76a1b19b-53bc-4737-9a70-1d52ce37bbb5
[alia2@localhost ~]$ sudo swapon /dev/sdb2
```

#### 1.5. Configure the newly created XFS file system to persistently mount at /backup

```bash
[alia2@localhost ~]$ sudo blkid
/dev/sda1: UUID="b4841cad-7301-444c-a240-fb87f7b1f3c6" TYPE="xfs"
/dev/sda2: UUID="R0LeQT-VcSi-mJwe-lBTc-IL9x-Q1Sw-U2nR1Y" TYPE="LVM2_member"
/dev/sdb1: LABEL="XFS" UUID="f87af91c-9246-4b90-8def-d707b207c9f8" TYPE="xfs" PARTLABEL="Linux filesystem" PARTUUID="b34b6850-cffc-4a4f-9794-0d08f2df92fa"
/dev/sdb2: UUID="76a1b19b-53bc-4737-9a70-1d52ce37bbb5" TYPE="swap" PARTLABEL="Linux swap" PARTUUID="ef77ec5e-aa64-4ef9-9153-fab30ca44f38"
/dev/mapper/centos-root: UUID="b1e61891-b20c-49cc-8b0e-38bd5af90542" TYPE="xfs"
/dev/mapper/centos-swap: UUID="d7576143-526e-4d40-b4ab-446f2bbbdef7" TYPE="swap"
[alia2@localhost ~]$ sudo mkdir /backup
[alia2@localhost ~]$ sudo mount /dev/sdb1 /backup
[alia2@localhost ~]$ sudo nano /etc/fstab
#
# /etc/fstab
# Created by anaconda on Sun Nov 28 23:14:47 2021
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
/dev/mapper/centos-root /                       xfs     defaults        0 0
UUID=b4841cad-7301-444c-a240-fb87f7b1f3c6 /boot                   xfs     default$
/dev/mapper/centos-swap swap                    swap    defaults        0 0
/dev/sdb1               /backup                 xfs     defaults        0 0
```

#### 1.6. Configure the newly created swap space to be enabled at boot

```bash
[alia2@localhost ~]$ sudo nano /etc/fstab
#added
/dev/sdb2               swap                    swap    defaults        0 0
[alia2@localhost ~]$ sudo mount -fav
```

#### 1.7. Reboot your host and verify that /dev/sdc1 is mounted at /backup and that your swap partition  (/dev/sdc2) is enabled

```bash
[alia2@localhost ~]$ reboot
[alia2@localhost ~]$ df
Filesystem              1K-blocks    Used Available Use% Mounted on
devtmpfs                   929104       0    929104   0% /dev
tmpfs                      940956       0    940956   0% /dev/shm
tmpfs                      940956    8800    932156   1% /run
tmpfs                      940956       0    940956   0% /sys/fs/cgroup
/dev/mapper/centos-root  16152576 1778964  14373612  12% /
/dev/sdb1                 2086912   32992   2053920   2% /backup
/dev/sda1                 1038336  198324    840012  20% /boot
tmpfs                      188192       0    188192   0% /run/user/1000
[alia2@localhost ~]$ cat /proc/swaps
Filename                                Type            Size    Used    Priority
/dev/dm-1                               partition       1912828 0       -2
/dev/sdb2                               partition       524284  0       -3
```

### 2. LVM. Imagine you're running out of space on your root device. As we found out during the lesson default CentOS installation should already have LVM, means you can easily extend size of your root device. So what are you waiting for? Just do it!
#### 2.1. Create 2GB partition on /dev/sdc of type "Linux LVM"

```bash
[alia2@localhost ~]$ sudo fdisk /dev/sdb
[sudo] password for alia2:
WARNING: fdisk GPT support is currently new, and therefore in an experimental phase. Use at your own discretion.
Welcome to fdisk (util-linux 2.23.2).

Changes will remain in memory only, until you decide to write them.
Be careful before using the write command.


Command (m for help):  n
Partition number (3-128, default 3):
First sector (34-10485726, default 5244928):
Last sector, +sectors or +size{K,M,G,T,P} (5244928-10485726, default 10485726): +2G
Created partition 3


Command (m for help): t
Partition number (1-3, default 3):
Partition type (type L to list all types): 31
Changed type of partition 'Linux filesystem' to 'Linux LVM'
Command (m for help): p

Disk /dev/sdb: 5368 MB, 5368709120 bytes, 10485760 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: gpt
Disk identifier: C44F20C1-3905-4219-B4E3-2BB938E13DC2


#         Start          End    Size  Type            Name
 1         2048      4196351      2G  Linux filesyste Linux filesystem
 2      4196352      5244927    512M  Linux swap      Linux swap
 3      5244928      9439231      2G  Linux LVM
[alia2@localhost ~]$ sudo partprobe
```

#### 2.2. Initialize the partition as a physical volume (PV)

```bash
[alia2@localhost ~]$ sudo pvcreate /dev/sdb3
  Physical volume "/dev/sdb3" successfully created.
```

#### 2.3. Extend the volume group (VG) of your root device using your newly created PV

```bash
[alia2@localhost ~]$ df /root
Filesystem              1K-blocks    Used Available Use% Mounted on
/dev/mapper/centos-root  16152576 1778840  14373736  12% /
[alia2@localhost ~]$ sudo lvs -o +devices /dev/mapper/centos-root
  LV   VG     Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert Devices
  root centos -wi-ao---- 15.41g
  /dev/sda2(467)
[alia2@localhost ~]$ sudo vgextend centos /dev/sdb3
  Volume group "centos" successfully extended
```

#### 2.4. Extend your root logical volume (LV) by 1GB, leaving other 1GB unassigned

```bash
[alia2@localhost ~]$ df -h
Filesystem               Size  Used Avail Use% Mounted on
devtmpfs                 908M     0  908M   0% /dev
tmpfs                    919M     0  919M   0% /dev/shm
tmpfs                    919M  8.7M  911M   1% /run
tmpfs                    919M     0  919M   0% /sys/fs/cgroup
/dev/mapper/centos-root   16G  1.7G   14G  12% /
/dev/sdb1                2.0G   33M  2.0G   2% /backup
/dev/sda1               1014M  194M  821M  20% /boot
tmpfs                    184M     0  184M   0% /run/user/1000
[alia2@localhost ~]$ sudo lvextend -L+1G /dev/centos/root
  Size of logical volume centos/root changed from 15.41 GiB (3946 extents) to 16.41 GiB (4202 extents).
  Logical volume centos/root successfully resized.
```

#### 2.5. Check current disk space usage of your root device

```bash
[alia2@localhost ~]$ sudo lvdisplay
  --- Logical volume ---
  LV Path                /dev/centos/swap
  LV Name                swap
  VG Name                centos
  LV UUID                G5qd0t-zkt5-bHk1-7cb4-G37S-2HvO-uxE9bH
  LV Write Access        read/write
  LV Creation host, time localhost, 2021-11-28 15:14:45 -0500
  LV Status              available
  # open                 2
  LV Size                1.82 GiB
  Current LE             467
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:1

  --- Logical volume ---
  LV Path                /dev/centos/root
  LV Name                root
  VG Name                centos
  LV UUID                ozStWM-f39n-IiOf-h2Jz-ecYI-BN5b-XlnYxP
  LV Write Access        read/write
  LV Creation host, time localhost, 2021-11-28 15:14:46 -0500
  LV Status              available
  # open                 1
  LV Size                16.41 GiB
  Current LE             4202
  Segments               2
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:0
```

#### 2.6. Extend your root device filesystem to be able to use additional free space of root LV

```bash
[alia2@localhost ~]$ sudo xfs_growfs /dev/centos/root
meta-data=/dev/mapper/centos-root isize=512    agcount=4, agsize=1010176 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0 spinodes=0
data     =                       bsize=4096   blocks=4040704, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal               bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 4040704 to 4302848
[alia2@localhost ~]$ df -h
Filesystem               Size  Used Avail Use% Mounted on
devtmpfs                 908M     0  908M   0% /dev
tmpfs                    919M     0  919M   0% /dev/shm
tmpfs                    919M  8.7M  911M   1% /run
tmpfs                    919M     0  919M   0% /sys/fs/cgroup
/dev/mapper/centos-root   17G  1.7G   15G  11% /
/dev/sdb1                2.0G   33M  2.0G   2% /backup
/dev/sda1               1014M  194M  821M  20% /boot
tmpfs                    184M     0  184M   0% /run/user/1000
```

#### 2.7. Verify that after reboot your root device is still 1GB bigger than at 2.5.

```bash
[alia2@localhost ~]$ reboot
[alia2@localhost ~]$ df -h
Filesystem               Size  Used Avail Use% Mounted on
devtmpfs                 908M     0  908M   0% /dev
tmpfs                    919M     0  919M   0% /dev/shm
tmpfs                    919M  8.6M  911M   1% /run
tmpfs                    919M     0  919M   0% /sys/fs/cgroup
/dev/mapper/centos-root   17G  1.7G   15G  11% /
/dev/sdb1                2.0G   33M  2.0G   2% /backup
/dev/sda1               1014M  194M  821M  20% /boot
tmpfs                    184M     0  184M   0% /run/user/1000
```
