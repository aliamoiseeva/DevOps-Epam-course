# Homework

## Repositories and Packages

#### 1. Download sysstat package.

```bash
[alia2@localhost ~]$ wget http://mirror.centos.org/centos/7/os/x86_64/Packages/sysstat-10.1.5-19.el7.x86_64.rpm
--2021-12-19 05:26:26--  http://mirror.centos.org/centos/7/os/x86_64/Packages/sysstat-10.1.5-19.el7.x86_64.rpm
Resolving mirror.centos.org (mirror.centos.org)... 141.105.67.224, 2a09:2dc0:0:5::10
Connecting to mirror.centos.org (mirror.centos.org)|141.105.67.224|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 323020 (315K) [application/x-rpm]
Saving to: ‘sysstat-10.1.5-19.el7.x86_64.rpm.1’

100%[==============================================================================>] 323,020      381KB/s   in 0.8s

2021-12-19 05:26:27 (381 KB/s) - ‘sysstat-10.1.5-19.el7.x86_64.rpm.1’ saved [323020/323020]

```

#### 2. Get information from downloaded sysstat package file.

```bash
[alia2@localhost ~]$ sudo rpm -qip sysstat-10.1.5-19.el7.x86_64.rpm
Name        : sysstat
Version     : 10.1.5
Release     : 19.el7
Architecture: x86_64
Install Date: (not installed)
Group       : Applications/System
Size        : 1172488
License     : GPLv2+
Signature   : RSA/SHA256, Fri 03 Apr 2020 05:08:48 PM CDT, Key ID 24c6a8a7f4a80eb5
Source RPM  : sysstat-10.1.5-19.el7.src.rpm
Build Date  : Wed 01 Apr 2020 12:36:37 AM CDT
Build Host  : x86-01.bsys.centos.org
Relocations : (not relocatable)
Packager    : CentOS BuildSystem <http://bugs.centos.org>
Vendor      : CentOS
URL         : http://sebastien.godard.pagesperso-orange.fr/
Summary     : Collection of performance monitoring tools for Linux
Description :
The sysstat package contains sar, sadf, mpstat, iostat, pidstat, nfsiostat-sysstat,
tapestat, cifsiostat and sa tools for Linux.
The sar command collects and reports system activity information. This
information can be saved in a file in a binary format for future inspection. The
statistics reported by sar concern I/O transfer rates, paging activity,
process-related activities, interrupts, network activity, memory and swap space
utilization, CPU utilization, kernel activities and TTY statistics, among
others. Both UP and SMP machines are fully supported.
The sadf command may be used to display data collected by sar in various formats
(CSV, XML, etc.).
The iostat command reports CPU utilization and I/O statistics for disks.
The tapestat command reports statistics for tapes connected to the system.
The mpstat command reports global and per-processor statistics.
The pidstat command reports statistics for Linux tasks (processes).
The nfsiostat-sysstat command reports I/O statistics for network file systems.
The cifsiostat command reports I/O statistics for CIFS file systems.
```

#### 3. Install sysstat package and get information about files installed by this package.

```bash
[alia2@localhost ~]$ sudo rpm -ivh sysstat-10.1.5-19.el7.x86_64.rpm
Preparing...                          ################################# [100%]
Updating / installing...
   1:sysstat-10.1.5-19.el7            ################################# [100%]
```

#### verifying sysstat installation

```bash
[alia2@localhost ~]$ mpstat -V
sysstat version 10.1.5
(C) Sebastien Godard (sysstat <at> orange.fr)
```

#### - Add NGINX repository (need to find repository config on https://www.nginx.com/) and complete the following tasks using yum:

```bash
[alia2@localhost ~]$ sudo yum install yum-utils
[alia2@localhost ~]$ sudo nano /etc/yum.repos.d/nginx.repo
[nginx-stable]
name=nginx stable repo
baseurl=http://nginx.org/packages/centos/$releasever/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[nginx-mainline]
name=nginx mainline repo
baseurl=http://nginx.org/packages/mainline/centos/$releasever/$basearch/
gpgcheck=1
enabled=0
gpgkey=https://nginx.org/keys/nginx_signing.key
module_hotfixes=true

[alia2@localhost ~]$ sudo yum-config-manager --enable nginx-mainline
```

#### 1. Check if NGINX repository enabled or not.

```bash
[alia2@localhost ~]$ yum repolist
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirrors.powernet.com.ru
 * extras: mirrors.powernet.com.ru
 * updates: mirrors.powernet.com.ru
nginx-mainline                                                                                   | 2.9 kB  00:00:00
nginx-stable                                                                                     | 2.9 kB  00:00:00
nginx-mainline/7/x86_64/primary_db                                                               | 227 kB  00:00:00
repo id                                                    repo name                                              status
base/7/x86_64                                              CentOS-7 - Base                                        10,072
extras/7/x86_64                                            CentOS-7 - Extras                                         500
nginx-mainline/7/x86_64                                    nginx mainline repo                                       875
nginx-stable/7/x86_64                                      nginx stable repo                                         256
updates/7/x86_64                                           CentOS-7 - Updates                                      3,190
repolist: 14,893
```

#### 2. Install NGINX.

```bash
[alia2@localhost ~]$ sudo yum install nginx
```

#### 3. Check yum history and undo NGINX installation.

```bash
[alia2@localhost ~]$ sudo yum history list
[sudo] password for alia2:
Loaded plugins: fastestmirror
ID     | Login user               | Date and time    | Action(s)      | Altered
-------------------------------------------------------------------------------
    15 | alia2 <alia2>            | 2021-12-19 07:29 | Install        |    1
    14 | alia2 <alia2>            | 2021-12-19 07:19 | Install        |    1
    13 | alia2 <alia2>            | 2021-12-19 07:17 | Erase          |    1
    12 | alia2 <alia2>            | 2021-12-19 07:08 | I, U           |  105
    11 | alia2 <alia2>            | 2021-12-19 07:00 | Erase          |    1
    10 | alia2 <alia2>            | 2021-12-19 06:55 | Install        |    1 EE
     9 | alia2 <alia2>            | 2021-12-19 06:52 | I, U           |    5  <
     8 | alia2 <alia2>            | 2021-12-19 05:24 | Erase          |    1 >
     7 | alia2 <alia2>            | 2021-12-19 04:50 | Install        |    2
     6 | alia2 <alia2>            | 2021-12-15 10:55 | Install        |   29
     5 | alia2 <alia2>            | 2021-12-09 11:32 | Install        |    1
     4 | alia2 <alia2>            | 2021-12-04 01:26 | Install        |    1
     3 | alia2 <alia2>            | 2021-11-29 13:04 | Install        |    1
     2 | alia2 <alia2>            | 2021-11-28 17:44 | Install        |    1
     1 | System <unset>           | 2021-11-28 15:14 | Install        |  302
Warning: RPMDB altered outside of yum.
history list

[alia2@localhost ~]$ sudo yum history package-info nginx
[sudo] password for alia2:
Loaded plugins: fastestmirror
Transaction ID : 10
Begin time     : Sun Dec 19 06:55:57 2021
Package        : nginx-1:1.21.4-1.el7.ngx.x86_64
State          : Install
Size           : 2,908,779
Build host     : ip-10-1-17-68.eu-central-1.compute.internal
Build time     : Tue Nov  2 11:06:33 2021
Vendor         : NGINX Packaging <nginx-packaging@f5.com>
License        : 2-clause BSD-like license
URL            : https://nginx.org/
Source RPM     : nginx-1.21.4-1.el7.ngx.src.rpm
Commit Time    : Tue Nov  2 08:00:00 2021
Committer      : Konstantin Pavlov <thresh@nginx.com>
Reason         : user
Command Line   : install nginx
From repo      : nginx-mainline
Installed by   : alia2 <alia2>
Warning: RPMDB altered outside of yum.
history package-info

[alia2@localhost ~]$ sudo yum history undo 10
Loaded plugins: fastestmirror
Undoing transaction 10, from Sun Dec 19 06:55:57 2021
    Install nginx-1:1.21.4-1.el7.ngx.x86_64 @nginx-mainline
No package matched to remove: nginx-1:1.21.4-1.el7.ngx
history undo
```

#### 4. Disable NGINX repository.

```bash
[alia2@localhost ~]$ sudo yum-config-manager --disable repository nginx-mainline
```

#### 5. Remove sysstat package installed in the first task.

```bash
[alia2@localhost ~]$ sudo yum remove sysstat
Loaded plugins: fastestmirror
Resolving Dependencies
--> Running transaction check
---> Package sysstat.x86_64 0:10.1.5-19.el7 will be erased
--> Finished Dependency Resolution

Dependencies Resolved

========================================================================================================================
 Package                   Arch                     Version                           Repository                   Size
========================================================================================================================
Removing:
 sysstat                   x86_64                   10.1.5-19.el7                     installed                   1.1 M

Transaction Summary
========================================================================================================================
Remove  1 Package

Installed size: 1.1 M
Is this ok [y/N]: y
Downloading packages:
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction
Warning: RPMDB altered outside of yum.
  Erasing    : sysstat-10.1.5-19.el7.x86_64                                                                         1/1
  Verifying  : sysstat-10.1.5-19.el7.x86_64                                                                         1/1

Removed:
  sysstat.x86_64 0:10.1.5-19.el7

Complete!
```

#### 6. Install EPEL repository and get information about it.

```bash
[alia2@localhost ~]$ sudo yum -y install epel-release
[alia2@localhost ~]$ sudo yum info epel-release
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.docker.ru
 * epel: repo.fedora.md
 * extras: mirror.corbina.net
 * updates: mirror.corbina.net
Installed Packages
Name        : epel-release
Arch        : noarch
Version     : 7
Release     : 14
Size        : 25 k
Repo        : installed
From repo   : epel
Summary     : Extra Packages for Enterprise Linux repository configuration
URL         : http://download.fedoraproject.org/pub/epel
License     : GPLv2
Description : This package contains the Extra Packages for Enterprise Linux (EPEL) repository
            : GPG key as well as configuration for yum.
```

#### 7. Find how much packages provided exactly by EPEL repository.
(first five lines of output contented unnecessary output)
```bash
[alia2@localhost ~]$ sudo yum --disablerepo="*" --enablerepo="epel" list available | tail -n +5 | wc -l
13907
```

#### 8. Install ncdu package from EPEL repo.

```bash
[alia2@localhost ~]$ sudo yum --enablerepo="epel" install ncdu
```

## Work with files

#### 1. Find all regular files below 100 bytes inside your home directory.

```bash
[alia2@localhost /]$ sudo find /home/alia2 -size -100c -type f
[sudo] password for alia2:
/home/alia2/.bash_logout
/home/alia2/.ssh/config
/home/alia2/a
/home/alia2/temp
/home/alia2/.local/share/mc/history
/home/alia2/dz/text.txt
/home/alia2/hello
/home/alia2/development/scripts/die
```

#### 2. Find an inode number and a hard links count for the root directory. The hard link count should be about 17. Why?

```bash
[alia2@localhost ~]$ stat /
  File: ‘/’
  Size: 258             Blocks: 0          IO Block: 4096   directory
Device: fd00h/64768d    Inode: 64          Links: 19
Access: (0555/dr-xr-xr-x)  Uid: (    0/    root)   Gid: (    0/    root)
Context: system_u:object_r:root_t:s0
Access: 2021-12-23 09:12:22.702000000 -0500
Modify: 2021-12-13 20:54:11.743000000 -0500
Change: 2021-12-13 20:54:11.743000000 -0500
 Birth: -
```

##### because 17 directories are used by default

#### 3. Check what inode numbers have "/" and "/boot" directory. Why?

```bash
[alia2@localhost ~]$ stat /
  File: ‘/’
  Size: 258             Blocks: 0          IO Block: 4096   directory
Device: fd00h/64768d    Inode: 64          Links: 19
Access: (0555/dr-xr-xr-x)  Uid: (    0/    root)   Gid: (    0/    root)
Context: system_u:object_r:root_t:s0
Access: 2021-12-21 11:09:08.634000000 -0500
Modify: 2021-12-13 20:54:11.743000000 -0500
Change: 2021-12-13 20:54:11.743000000 -0500
 Birth: -
[alia2@localhost ~]$ stat /boot
  File: ‘/boot’
  Size: 4096            Blocks: 8          IO Block: 4096   directory
Device: 801h/2049d      Inode: 64          Links: 5
Access: (0555/dr-xr-xr-x)  Uid: (    0/    root)   Gid: (    0/    root)
Context: system_u:object_r:boot_t:s0
Access: 2021-12-21 14:37:23.191000000 -0500
Modify: 2021-12-21 11:10:00.840000000 -0500
Change: 2021-12-21 11:10:00.840000000 -0500
 Birth: -
```

##### /boot is a different file system of the same type (xfs), and it's the root directory of this file system, so it's very likely that it uses the same inode number as /. 

#### 4. Check the root directory space usage by du command. Compare it with an information from df. If you find differences, try to find out why it happens.

```bash
[alia2@localhost ~]$ df -h
Filesystem               Size  Used Avail Use% Mounted on
devtmpfs                 908M     0  908M   0% /dev
tmpfs                    919M     0  919M   0% /dev/shm
tmpfs                    919M  8.6M  911M   1% /run
tmpfs                    919M     0  919M   0% /sys/fs/cgroup
/dev/mapper/centos-root   16G  1.7G   14G  11% /
/dev/sda1               1014M  194M  821M  20% /boot
tmpfs                    184M     0  184M   0% /run/user/1000
[alia2@localhost ~]$ sudo du -hcs /
[sudo] password for alia2:
du: cannot access ‘/proc/2262/task/2262/fd/4’: No such file or directory
du: cannot access ‘/proc/2262/task/2262/fdinfo/4’: No such file or directory
du: cannot access ‘/proc/2262/fd/3’: No such file or directory
du: cannot access ‘/proc/2262/fdinfo/3’: No such file or directory
1.8G    /
1.8G    total
```

##### du queries directly for each file found in the partition, and df queries the filesystem

#### 5. Check disk space usage of /var/log directory using ncdu

```bash
[alia2@localhost ~]$ ncdu -x /var/log
 Total disk usage:   9.3 MiB  Apparent size:   9.6 MiB  Items: 59
```
