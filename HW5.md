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
[Unix]
Description=First daemon
[Service]
ExecStart=/home/alia2/firstd.sh
[alia2@localhost ~]$ sudo nano /etc/systemd/system/secondd.service
[Unix]
Description=Second daemon
[Service]
Type=oneshot
ExecStart=/home/alia2/secondd.sh
```

### 2. Make the second depended on the first one (should start only after the first)

```bash
[alia2@localhost ~]$ sudo nano /etc/systemd/system/secondd.service
[Unix]
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
[Unix]
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
● firstd.service
   Loaded: loaded (/etc/systemd/system/firstd.service; static; vendor preset: disabled)
   Active: inactive (dead)

Dec 16 23:12:23 localhost.localdomain systemd[1]: [/etc/systemd/system/firstd.service:1] Unknown section 'Unix'. ...ing.
Dec 16 23:12:23 localhost.localdomain systemd[1]: Started firstd.service.
Dec 16 23:15:25 localhost.localdomain systemd[1]: [/etc/systemd/system/firstd.service:1] Unknown section 'Unix'. ...ing.
Dec 16 23:16:59 localhost.localdomain systemd[1]: [/etc/systemd/system/firstd.service:1] Unknown section 'Unix'. ...ing.
Dec 16 23:16:59 localhost.localdomain systemd[1]: [/etc/systemd/system/firstd.service:1] Unknown section 'Unix'. ...ing.
Dec 16 23:16:59 localhost.localdomain systemd[1]: [/etc/systemd/system/firstd.service:1] Unknown section 'Unix'. ...ing.
Dec 16 23:18:24 localhost.localdomain systemd[1]: [/etc/systemd/system/firstd.service:1] Unknown section 'Unix'. ...ing.
Dec 16 23:18:24 localhost.localdomain systemd[1]: Started firstd.service.
Dec 16 23:18:49 localhost.localdomain systemd[1]: [/etc/systemd/system/firstd.service:1] Unknown section 'Unix'. ...ing.
Hint: Some lines were ellipsized, use -l to show in full.

[alia2@localhost ~]$ sudo systemctl status secondd.service
● secondd.service
   Loaded: loaded (/etc/systemd/system/secondd.service; static; vendor preset: disabled)
   Active: inactive (dead)

Dec 16 23:14:52 localhost.localdomain systemd[1]: [/etc/systemd/system/secondd.service:1] Unknown section 'Unix'....ing.
Dec 16 23:17:16 localhost.localdomain systemd[1]: [/etc/systemd/system/secondd.service:1] Unknown section 'Unix'....ing.
Dec 16 23:17:16 localhost.localdomain systemd[1]: [/etc/systemd/system/secondd.service:1] Unknown section 'Unix'....ing.
Dec 16 23:17:19 localhost.localdomain systemd[1]: [/etc/systemd/system/secondd.service:1] Unknown section 'Unix'....ing.
Dec 16 23:17:19 localhost.localdomain systemd[1]: [/etc/systemd/system/secondd.service:1] Unknown section 'Unix'....ing.
Dec 16 23:17:19 localhost.localdomain systemd[1]: [/etc/systemd/system/secondd.service:1] Unknown section 'Unix'....ing.
Dec 16 23:18:31 localhost.localdomain systemd[1]: [/etc/systemd/system/secondd.service:1] Unknown section 'Unix'....ing.
Dec 16 23:18:31 localhost.localdomain systemd[1]: Starting secondd.service...
Dec 16 23:18:31 localhost.localdomain systemd[1]: Started secondd.service.
Dec 16 23:18:35 localhost.localdomain systemd[1]: [/etc/systemd/system/secondd.service:1] Unknown section 'Unix'....ing.
Hint: Some lines were ellipsized, use -l to show in full.

[alia2@localhost ~]$ sudo systemctl status secondd.timer
● secondd.timer
   Loaded: loaded (/etc/systemd/system/secondd.timer; static; vendor preset: disabled)
   Active: active (elapsed) since Thu 2021-12-16 23:18:35 MSK; 26s ago

Dec 16 23:18:35 localhost.localdomain systemd[1]: Started secondd.timer.
[alia2@localhost ~]$ sudo systemctl list-timers --all
NEXT                         LEFT     LAST                         PASSED       UNIT                         ACTIVATES
n/a                          n/a      Thu 2021-12-16 23:14:52 MSK  4min 52s ago secondd.timer                secondd.ser
Fri 2021-12-17 23:04:29 MSK  23h left Thu 2021-12-16 23:04:29 MSK  15min ago    systemd-tmpfiles-clean.timer systemd-tmp
n/a                          n/a      n/a                          n/a          systemd-readahead-done.timer systemd-rea

3 timers listed.

[1]+  Stopped                 sudo systemctl list-timers --all

[alia2@localhost ~]$ cat /tmp/homework
1
```

### 5. Stop all daemons and timer

```bash
[alia2@localhost ~]$ sudo systemctl stop firstd.service
[alia2@localhost ~]$ sudo systemctl stop secondd.service
Warning: Stopping secondd.service, but it can still be activated by:
  secondd.timer
[alia2@localhost ~]$ sudo systemctl stop secondd.timer
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
