# Homework

### 1. add secondary ip address to you second network interface enp0s8. Each point must be presented with commands and showing that new address was applied to the interface. To repeat adding address for points 2 and 3 address must be deleted (please add deleting address to you homework log) Methods:

   #### 1.1 using ip utility (stateless)

```bash
[alia2@localhost ~]$ sudo ip addr add 192.168.122.197/24 dev enp0s8
[alia2@localhost ~]$  ip addr show enp0s8
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:b2:db:28 brd ff:ff:ff:ff:ff:ff
    inet 172.20.10.7/28 brd 172.20.10.15 scope global noprefixroute dynamic enp0s8
       valid_lft 85976sec preferred_lft 85976sec
    inet 192.168.122.197/24 scope global enp0s8
       valid_lft forever preferred_lft forever
    inet6 fe80::1dfc:3a01:b4e4:5d6f/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
[alia2@localhost ~]$ sudo ip addr del 192.168.122.197/24 dev enp0s8
[alia2@localhost ~]$  ip addr show enp0s8
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:b2:db:28 brd ff:ff:ff:ff:ff:ff
    inet 172.20.10.7/28 brd 172.20.10.15 scope global noprefixroute dynamic enp0s8
       valid_lft 85925sec preferred_lft 85925sec
    inet6 fe80::1dfc:3a01:b4e4:5d6f/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```

   #### 1.2 using centos network configuration file (statefull)

```bash
[alia2@localhost ~]$ sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s8:1
DEVICE=enp0s8:1
Type=Ethernet
ONBOOT=yes
NM_CONTROLLED=no
BOOTPROTO=none
IPADDR=192.168.122.197
PREFIX=24
[alia2@sib-nsit357nb-vm1 ~]$ systemctl restart network
[alia2@localhost ~]$ ip addr show enp0s8
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:b2:db:28 brd ff:ff:ff:ff:ff:ff
    inet 192.168.122.197/24 brd 192.168.122.255 scope global noprefixroute enp0s8
       valid_lft forever preferred_lft forever
    inet 172.20.10.7/28 brd 172.20.10.15 scope global noprefixroute dynamic enp0s8
       valid_lft 86283sec preferred_lft 86283sec
    inet6 fe80::1dfc:3a01:b4e4:5d6f/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
[alia2@localhost ~]$ sudo nano /etc/sysconfig/network-scripts/ifcfg-enp0s8:1 # removing all that was added earlier
[alia2@localhost ~]$  ip addr show enp0s8
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:b2:db:28 brd ff:ff:ff:ff:ff:ff
    inet 172.20.10.7/28 brd 172.20.10.15 scope global noprefixroute dynamic enp0s8
       valid_lft 85925sec preferred_lft 85925sec
    inet6 fe80::1dfc:3a01:b4e4:5d6f/64 scope link noprefixroute
       valid_lft forever preferred_lft forever
```

   #### 1.3 using nmcli utility (statefull)

```bash
[alia2@localhost ~]$ nmcli connection show
NAME    UUID                                  TYPE      DEVICE
enp0s3  839c2ef7-ea1e-4fc3-979d-637e614e3c56  ethernet  enp0s3
enp0s8  e6c1a12c-b4d9-45e9-b7d4-9b16a02ef4ed  ethernet  enp0s8
[alia2@localhost ~]$ sudo nmcli con mod enp0s8 +ipv4.addresses "192.168.122.197/24"
[alia2@localhost ~]$ sudo reboot
[alia2@localhost ~]$ ip addr | grep 'inet '
    inet 127.0.0.1/8 scope host lo
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic enp0s3
    inet 192.168.122.197/24 brd 192.168.122.255 scope global noprefixroute enp0s8
    inet 172.20.10.7/28 brd 172.20.10.15 scope global noprefixroute dynamic enp0s8
```

### 2. You should have a possibility to use ssh client to connect to your node using new address from previous step. Run tcpdump in separate tmux session or separate connection before starting ssh client and capture packets that are related to this ssh connection. Find packets that are related to TCP session establish.
### 3. Close session. Find in tcpdump output packets that are related to TCP session closure.

```bash
[alia2@localhost ~]$ tmux new-session -s HW
[alia2@localhost ~]$ sudo tcpdump -i lo -w /home/alia2/log.c│[alia2@localhost ~]$ ssh alia2@192.168.122.197
ap                                                          │alia2@192.168.122.197's password:
[sudo] password for alia2:                                  │Last login: Tue Dec 28 00:32:30 2021 from 192.168.122.197
tcpdump: listening on lo, link-type EN10MB (Ethernet), captu│[alia2@localhost ~]$ logout
re size 262144 bytes                                        │Connection to 192.168.122.197 closed.
^C96 packets captured                                       │
192 packets received by filter                              │
0 packets dropped by kernel
```

#### terminal (Windows)

```bash
PS C:\Program Files\PuTTY> .\pscp -l alia2 172.20.10.7:/home/alia2/log.cap D:\
```

##### Start SSH session (establishing a connection) ![start SSH session](https://user-images.githubusercontent.com/95036108/147680087-0123f5cd-16c7-4f21-81fd-7772c9757c2d.PNG)
##### The end SSH session ![final SSH seccion](https://user-images.githubusercontent.com/95036108/147680324-7bd36a30-d503-4a72-934e-9c38c302d324.PNG)

### 4. run tcpdump and request any http site in separate session. Find HTTP request and answer packets with ASCII data in it.  Tcpdump command must be as strict as possible to capture only needed packages for this http request.

```bash
[alia2@sib-nsit357nb-vm1 ~]$ tmux
```

#### first window

```bash
[alia2@sib-nsit357nb-vm1 ~]$ sudo tcpdump -A tcp port 80 -w /home/alia2/log2.pcap
tcpdump: listening on enp0s3, link-type EN10MB (Ethernet), capture size 262144 bytes
^C26 packets captured
26 packets received by filter
0 packets dropped by kernel
```

#### second window

```bash
[alia2@sib-nsit357nb-vm1 ~]$ curl http://cs.mipt.ru/algo/lessons/lab2.html
```

#### terminal (Windows PC)

```bash
PS C:\Program Files\PuTTY> .\pscp -l alia2 172.20.10.7:/home/alia2/log2.pcap D:\
alia2@172.20.10.7's password:
log2.pcap                 | 156 kB | 156.0 kB/s | ETA: 00:00:00 | 100%
```

##### HTTP request and answer packets with ASCII data in it ![image](https://user-images.githubusercontent.com/95036108/147693188-f1a3fc7c-f080-402a-9f1b-0b7cb2d23095.png)

