# Homework

## ex1

```bash
[alia2@localhost ~]$ ls /usr/share/man/man?/*config*
/usr/share/man/man1/pkg-config.1.gz        /usr/share/man/man8/chkconfig.8.gz
/usr/share/man/man5/config.5ssl.gz         /usr/share/man/man8/grub2-mkconfig.8.gz
/usr/share/man/man5/config-util.5.gz       /usr/share/man/man8/iprconfig.8.gz
/usr/share/man/man5/selinux_config.5.gz    /usr/share/man/man8/lvm-config.8.gz
/usr/share/man/man5/ssh_config.5.gz        /usr/share/man/man8/lvmconfig.8.gz
/usr/share/man/man5/sshd_config.5.gz       /usr/share/man/man8/lvm-dumpconfig.8.gz
/usr/share/man/man5/x509v3_config.5ssl.gz  /usr/share/man/man8/sg_get_config.8.gz
/usr/share/man/man8/authconfig.8.gz        /usr/share/man/man8/sys-unconfig.8.gz
/usr/share/man/man8/authconfig-tui.8.gz
[alia2@localhost ~]$ ls /usr/share/man/man{1,7}/*system*
/usr/share/man/man1/systemctl.1.gz                  /usr/share/man/man1/systemd-machine-id-commit.1.gz
/usr/share/man/man1/systemd.1.gz                    /usr/share/man/man1/systemd-machine-id-setup.1.gz
/usr/share/man/man1/systemd-analyze.1.gz            /usr/share/man/man1/systemd-notify.1.gz
/usr/share/man/man1/systemd-ask-password.1.gz       /usr/share/man/man1/systemd-nspawn.1.gz
/usr/share/man/man1/systemd-bootchart.1.gz          /usr/share/man/man1/systemd-path.1.gz
/usr/share/man/man1/systemd-cat.1.gz                /usr/share/man/man1/systemd-run.1.gz
/usr/share/man/man1/systemd-cgls.1.gz               /usr/share/man/man1/systemd-tty-ask-password-agent.1.gz
/usr/share/man/man1/systemd-cgtop.1.gz              /usr/share/man/man7/lvmsystemid.7.gz
/usr/share/man/man1/systemd-delta.1.gz              /usr/share/man/man7/systemd.directives.7.gz
/usr/share/man/man1/systemd-detect-virt.1.gz        /usr/share/man/man7/systemd.generator.7.gz
/usr/share/man/man1/systemd-escape.1.gz             /usr/share/man/man7/systemd.index.7.gz
/usr/share/man/man1/systemd-firstboot.1.gz          /usr/share/man/man7/systemd.journal-fields.7.gz
/usr/share/man/man1/systemd-firstboot.service.1.gz  /usr/share/man/man7/systemd.special.7.gz
/usr/share/man/man1/systemd-inhibit.1.gz            /usr/share/man/man7/systemd.time.7.gz
```

## ex2

```bash
[alia2@localhost ~]$ find /usr/share/man -type f -name "*help*"
/usr/share/man/man1/help.1.gz
/usr/share/man/man5/firewalld.helper.5.gz
/usr/share/man/man8/mkhomedir_helper.8.gz
/usr/share/man/man8/pwhistory_helper.8.gz
/usr/share/man/man8/ssh-pkcs11-helper.8.gz
[alia2@localhost ~]$ find /usr/share/man -type f -name "conf*"
/usr/share/man/man5/config.5ssl.gz
/usr/share/man/man5/config-util.5.gz
[alia2@localhost ~]$ touch main.cpp
[alia2@localhost ~]$ ls
main.cpp
[alia2@localhost ~]$ find . -name "main.*" -exec rm -rf '{}' \;  # -exec flag can be used to execute something on found files. For example, for deleting them. 
[alia2@localhost ~]$ ls
```

## ex3

```bash
[alia2@localhost ~]$ head -7 /etc/yum.conf
[main]
cachedir=/var/cache/yum/$basearch/$releasever
keepcache=0
debuglevel=2
logfile=/var/log/yum.log
exactarch=1
obsoletes=1
[alia2@localhost ~]$ tail -2 /etc/fstab
UUID=b4841cad-7301-444c-a240-fb87f7b1f3c6 /boot                   xfs     defaults        0 0
/dev/mapper/centos-swap swap                    swap    defaults        0 0
[alia2@localhost ~]$ head -67 /etc/fstab | wc -l
11
```

## ex4

```bash
[alia2@localhost ~]$ touch file_name{1..3}.md
[alia2@localhost ~]$ ls
file_name1.md  file_name2.md  file_name3.md
[alia2@localhost ~]$ mv file_name1.{md,textdoc}
[alia2@localhost ~]$ mv file_name2{.md,}
[alia2@localhost ~]$ mv file_name3.md{,.latest}
[alia2@localhost ~]$ mv file_name1.{textdoc,txt}
[alia2@localhost ~]$ ls
file_name1.txt  file_name2  file_name3.md.latest
```

## ex5

```bash
[alia2@localhost ~]$ cd /mnt
[alia2@localhost mnt]$ cd ~
[alia2@localhost mnt]$ cd ../home/alia2
[alia2@localhost mnt]$ cd ~alia2
```

## ex6

```bash
[alia2@localhost ~]$ mkdir -p new in-process/tread{0..2} processed
[alia2@localhost ~]$ tree
.
├── file_name1.txt
├── file_name2
├── file_name3.md.latest
├── in-process
│   ├── tread0
│   ├── tread1
│   └── tread2
├── new
└── processed

6 directories, 3 files
[alia2@localhost ~]$ touch new/data{00..99}
[alia2@localhost ~]$ cp new/data{00..33} in-process/tread0
[alia2@localhost ~]$ cp new/data{34..66} in-process/tread1
[alia2@localhost ~]$ cp new/data{67..99} in-process/tread2
[alia2@localhost ~]$ ls in-process/tread0 in-process/tread1 in-process/tread2
in-process/tread0:
data00  data03  data06  data09  data12  data15  data18  data21  data24  data27  data30  data33
data01  data04  data07  data10  data13  data16  data19  data22  data25  data28  data31
data02  data05  data08  data11  data14  data17  data20  data23  data26  data29  data32

in-process/tread1:
data34  data37  data40  data43  data46  data49  data52  data55  data58  data61  data64
data35  data38  data41  data44  data47  data50  data53  data56  data59  data62  data65
data36  data39  data42  data45  data48  data51  data54  data57  data60  data63  data66

in-process/tread2:
data67  data70  data73  data76  data79  data82  data85  data88  data91  data94  data97
data68  data71  data74  data77  data80  data83  data86  data89  data92  data95  data98
data69  data72  data75  data78  data81  data84  data87  data90  data93  data96  data99
[alia2@localhost ~]$

#!/bin/bash

a=$(ls /home/alia2/new | wc -l)
b=$(ls /home/alia2/processed | wc -l)

if [ "$a" == "$b" ];
        then
                echo "deleting files in new folder"
                rm -rf /home/alia2/new/*
fi
```

## ex7

```bash
[alia2@localhost ~]$ a=1
[alia2@localhost ~]$ b=3
[alia2@localhost ~]$ echo file{$a..$b} | xargs -I@ bash -c 'echo @'
file1 file2 file3
```
