# Homework

## Processes

### 1. Run a sleep command three times at different intervals

```bash
[alia2@localhost ~]$ sleep 10000 &
[1] 2361
[alia2@localhost ~]$ sleep 100000 &
[2] 2362
[alia2@localhost ~]$ sleep 1000000 &
[3] 2363
```

### 2. Send a SIGSTOP signal to all of them in three different ways.

```bash
[alia2@localhost ~]$ kill -20 2361

[1]+  Stopped                 sleep 10000
[alia2@localhost ~]$ kill -SIGSTOP 2362

[2]+  Stopped                 sleep 100000
[alia2@localhost ~]$ fg %3
sleep 1000000
^Z
[3]+  Stopped                 sleep 1000000
```

### 3. Check their statuses with a job command

```bash
[alia2@localhost ~]$ jobs
[1]   Stopped                 sleep 10000
[2]-  Stopped                 sleep 100000
[3]+  Stopped                 sleep 1000000
```

### 4. Terminate one of them. (Any)

```bash
[alia2@localhost ~]$ kill -18 %1
[alia2@localhost ~]$ kill %1
[alia2@localhost ~]$ jobs
[1]   Terminated              sleep 10000
[2]-  Stopped                 sleep 100000
[3]+  Stopped                 sleep 1000000
```

### 5. To other send a SIGCONT in two different ways.

```bash
[alia2@localhost ~]$ kill -SIGCONT %2
[alia2@localhost ~]$ kill -18 2363
[alia2@localhost ~]$ jobs
[2]-  Running                 sleep 100000 &
[3]+  Running                 sleep 1000000 &
```

### 6. Kill one by PID and the second one by job ID

```bash
[alia2@localhost ~]$ kill 2362
[alia2@localhost ~]$ kill %3
[2]-  Terminated              sleep 100000
[3]+  Terminated              sleep 1000000
```

## systemd

### 1. Write two daemons: one should be a simple daemon and do sleep 10 after a start and then do echo 1 > /tmp/homework, the second one should be oneshot and do echo 2 > /tmp/homework without any sleep

```bash
[alia2@localhost ~]$ nano /home/alia2/firstd.sh
[alia2@localhost ~]$ nano /home/alia2/secondd.sh
[alia2@localhost ~]$ sudo nano /etc/systemd/system/firstd.service
[Unit]
Description=First daemon
[Service]
ExecStart=/home/alia2/firstd.sh
[alia2@localhost ~]$ sudo nano /etc/systemd/system/secondd.service
[Unit]
Description=Second daemon
[Service]
Type=oneshot
ExecStart=/home/alia2/secondd.sh
```

### 2. Make the second depended on the first one (should start only after the first)

```bash
[alia2@localhost ~]$ sudo nano /etc/systemd/system/secondd.service
[Unit]
Description=Second daemon
Requires=firstd.service
After=firstd.service
[Service]
Type=oneshot
ExecStart=/home/alia2/secondd.sh
```

### 3. Write a timer for the second one and configure it to run on 01.01.2019 at 00:00

```bash
[alia2@localhost ~]$ sudo nano /etc/systemd/system/secondd.timer
[Unit]
Description=Timer for the secondd.service
[Service]
OnCalendar=2019-01-01 00:00
Persistent=true
```

### 4. Start all daemons and timer, check their statuses, timer list and /tmp/homework

```bash
[alia2@localhost ~]$ sudo systemctl start firstd.service
[alia2@localhost ~]$ sudo systemctl start secondd.service
[alia2@localhost ~]$ sudo systemctl start secondd.timer
[alia2@localhost ~]$ sudo systemctl status firstd.service
[alia2@localhost ~]$ sudo systemctl status firstd.service secondd.service secondd.timer
● firstd.service - First daemon
   Loaded: loaded (/etc/systemd/system/firstd.service; static; vendor preset: disabled)
   Active: inactive (dead) since Tue 2021-12-21 11:12:21 CST; 17s ago
  Process: 8342 ExecStart=/home/alia2/firstd.sh (code=exited, status=0/SUCCESS)
 Main PID: 8342 (code=exited, status=0/SUCCESS)

Dec 21 11:12:11 localhost.localdomain systemd[1]: Started First daemon.

● secondd.service - Second daemon
   Loaded: loaded (/etc/systemd/system/secondd.service; static; vendor preset: disabled)
   Active: inactive (dead)

Dec 21 11:12:11 localhost.localdomain systemd[1]: Starting Second daemon...
Dec 21 11:12:11 localhost.localdomain systemd[1]: Started Second daemon.

● secondd.timer - Timer for second daemon
   Loaded: loaded (/etc/systemd/system/secondd.timer; static; vendor preset: disabled)
   Active: active (elapsed) since Tue 2021-12-21 11:12:19 CST; 19s ago

Dec 21 11:12:19 localhost.localdomain systemd[1]: Started Timer for second daemon.

[alia2@localhost ~]$ cat /tmp/homework
1
```

### 5. Stop all daemons and timer

```bash
[alia2@localhost ~]$ sudo systemctl stop firstd.service secondd.service secondd.timer
```

## cron/anacron

### 1. Create an anacron job which executes a script with echo Hello > /opt/hello and runs every 2 days

```bash
[alia2@localhost ~]$ nano hello.sh
#!/bin/bash

echo Hello > /opt/hello
```

#### Открываем файл конфигурации anacron и добавляем строчку "2 0 hello.anacron /bin/bash /root/hello.sh"

```bash
[alia2@localhost ~]$ sudo vi /etc/anacrontab
# /etc/anacrontab: configuration file for anacron

# See anacron(8) and anacrontab(5) for details.

SHELL=/bin/sh
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=root
# the maximal random delay added to the base delay of the jobs
RANDOM_DELAY=45
# the jobs will be started during the following hours only
START_HOURS_RANGE=3-22

#period in days   delay in minutes   job-identifier   command
1       5       cron.daily              nice run-parts /etc/cron.daily
7       25      cron.weekly             nice run-parts /etc/cron.weekly
@monthly 45     cron.monthly            nice run-parts /etc/cron.monthly
2       0       hello.anacron           /bin/bash /home/alia2/hello.sh
```

### 2. Create a cron job which executes the same command (will be better to create a script for this) and runs it in 1 minute after system boot.

```bash
[alia2@localhost ~]$ sudo crontab -e
@reboot root sleep 60 && /home/alia2/hello.sh
```

### 3. Restart your virtual machine and check previous job proper execution

```bash
[alia2@localhost ~]$ sudo shutdown
[root@localhost opt]# cat /opt/hello
Hello
```

## lsof

### 1. Run a sleep command, redirect stdout and stderr into two different files (both of them will be empty).

```bash
[alia2@localhost ~]$ sleep 10000 1>output 2>errors &
[1] 1667
```

### 2. Find with the lsof command which files this process uses, also find from which file it gain stdin.

```bash
[alia2@localhost ~]$ sleep 10000 1>output 2>errors &
[2] 1642
[alia2@localhost ~]$ lsof -p 1642
COMMAND  PID  USER   FD   TYPE DEVICE  SIZE/OFF     NODE NAME
sleep   1642 alia2  cwd    DIR  253,0      4096   956216 /home/alia2
sleep   1642 alia2  rtd    DIR  253,0       258       64 /
sleep   1642 alia2  txt    REG  253,0     33128 25277493 /usr/bin/sleep
sleep   1642 alia2  mem    REG  253,0 106172832 25173835 /usr/lib/locale/locale-archive
sleep   1642 alia2  mem    REG  253,0   2156272    40537 /usr/lib64/libc-2.17.so
sleep   1642 alia2  mem    REG  253,0    163312    40530 /usr/lib64/ld-2.17.so
sleep   1642 alia2    0u   CHR  136,0       0t0        3 /dev/pts/0
sleep   1642 alia2    1w   REG  253,0         0    19604 /home/alia2/output
sleep   1642 alia2    2w   REG  253,0         0    19606 /home/alia2/errors
```

### 3. List all ESTABLISHED TCP connections ONLY with lsof

```bash
[alia2@localhost ~]$ lsof -i TCP -s TCP:ESTABLISHED
```
