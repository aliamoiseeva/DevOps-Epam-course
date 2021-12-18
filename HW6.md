# Homework

## Task 1

### remotehost - 18.221.144.175 (public IP)
### webserver - 172.31.45.237 (private IP)

#### 1.1. SSH to remotehost using username and password provided to you in Slack. Log out from remotehost.

```bash
[alia2@localhost ~]$ ssh Aliia_Moiseeva@18.221.144.175
The authenticity of host '18.221.144.175 (18.221.144.175)' can't be established.
ECDSA key fingerprint is SHA256:Lqk214fPCrlvcsnoj1NGVS+Puxr7lGuEncIdALeLt78.
ECDSA key fingerprint is MD5:01:a1:c8:32:41:5c:9b:4b:0a:cc:f0:4b:7e:66:2b:38.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '18.221.144.175' (ECDSA) to the list of known hosts.
Aliia_Moiseeva@18.221.144.175's password:

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
No packages needed for security; 2 packages available
Run "sudo yum update" to apply all updates.
[Aliia_Moiseeva@ip-172-31-33-155 ~]$ logout
Connection to 18.221.144.175 closed.
```

#### 1.2. Generate new SSH key-pair on your localhost with name "hw-5" (keys should be created in ~/.ssh folder).

```bash
[alia2@localhost .ssh]$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/alia2/.ssh/id_rsa): hw-5
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in hw-5.
Your public key has been saved in hw-5.pub.
The key fingerprint is:
SHA256:qRTs7oROC3TsKGrIaX7YmHEbC5WbF7FmenoutQYdUvs alia2@localhost.localdomain
The key's randomart image is:
+---[RSA 2048]----+
|                 |
|     +           |
|    o *          |
|   = O . .       |
|  o @ * S        |
| + @ O E         |
|+ % & =          |
|oX @ B           |
|=.. B..          |
+----[SHA256]-----+
[alia2@localhost ~]$ ls .ssh
hw-5  hw-5.pub  known_hosts
```

#### 1.3. Set up key-based authentication, so that you can SSH to remotehost without password.

```bash
[alia2@localhost ~]$ ssh-copy-id -i ~/.ssh/hw-5.pub Aliia_Moiseeva@18.221.144.175
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/alia2/.ssh/hw-5.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
Aliia_Moiseeva@18.221.144.175's password:

Number of key(s) added: 1

Now try logging into the machine, with:   "ssh 'Aliia_Moiseeva@18.221.144.175'"
and check to make sure that only the key(s) you wanted were added.
```

#### 1.4. SSH to remotehost without password. Log out from remotehost.

```bash
[alia2@localhost ~]$ ssh -i /home/alia2/.ssh/hw-5 Aliia_Moiseeva@18.221.144.175
Last login: Sat Dec 18 18:08:10 2021 from 188.170.72.149

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
No packages needed for security; 4 packages available
Run "sudo yum update" to apply all updates.
[Aliia_Moiseeva@ip-172-31-33-155 ~]$ logout
Connection to 18.221.144.175 closed.
```

#### 1.5. Create SSH config file, so that you can SSH to remotehost simply running `ssh remotehost` command. As a result, provide output of command `cat ~/.ssh/config`.

```bash
[alia2@localhost .ssh]$ nano config
Host remotehost
        Hostname 18.221.144.175
        User Aliia_Moiseeva
        IdentityFile ~/.ssh/hw-5
[alia2@localhost .ssh]$ sudo chmod 644 config
[alia2@localhost .ssh]$ ssh remotehost
Last login: Sat Dec 18 18:18:13 2021 from 188.170.72.149

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

https://aws.amazon.com/amazon-linux-2/
No packages needed for security; 4 packages available
Run "sudo yum update" to apply all updates.
[Aliia_Moiseeva@ip-172-31-33-155 ~]$
```

#### 1.6. Using command line utility (curl or telnet) verify that there are some webserver running on port 80 of webserver.  Notice that webserver has a private network IP, so you can access it only from the same network (when you are on remotehost that runs in the same private network). Log out from remotehost.

```bash
[Aliia_Moiseeva@ip-172-31-33-155 ~]$ curl 172.31.45.237:80
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd"><html><head>
<meta http-equiv="content-type" content="text/html; charset=UTF-8">
                <title>Apache HTTP Server Test Page powered by CentOS</title>
                <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">

    <!-- Bootstrap -->
    <link href="/noindex/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="noindex/css/open-sans.css" type="text/css" />

<style type="text/css"><!--

body {
  font-family: "Open Sans", Helvetica, sans-serif;
  font-weight: 100;
  color: #ccc;
  background: rgba(10, 24, 55, 1);
  font-size: 16px;
}

h2, h3, h4 {
  font-weight: 200;
}

h2 {
  font-size: 28px;
}

.jumbotron {
  margin-bottom: 0;
  color: #333;
  background: rgb(212,212,221); /* Old browsers */
  background: radial-gradient(ellipse at center top, rgba(255,255,255,1) 0%,rgba(174,174,183,1) 100%); /* W3C */
}

.jumbotron h1 {
  font-size: 128px;
  font-weight: 700;
  color: white;
  text-shadow: 0px 2px 0px #abc,
               0px 4px 10px rgba(0,0,0,0.15),
               0px 5px 2px rgba(0,0,0,0.1),
               0px 6px 30px rgba(0,0,0,0.1);
}

.jumbotron p {
  font-size: 28px;
  font-weight: 100;
}

.main {
   background: white;
   color: #234;
   border-top: 1px solid rgba(0,0,0,0.12);
   padding-top: 30px;
   padding-bottom: 40px;
}

.footer {
   border-top: 1px solid rgba(255,255,255,0.2);
   padding-top: 30px;
}

    --></style>
</head>
<body>
  <div class="jumbotron text-center">
    <div class="container">
          <h1>Hello!</h1>
                <p class="lead">You are here because you're probably a DevOps courses member. In that case you should open <a href="https://www.youtube.com/watch?v=dQw4w9WgXcQ"> THIS LINK </a></p>
                </div>
  </div>
</body></html>
```

#### 1.7. Using SSH setup port forwarding, so that you can reach webserver from your localhost (choose any free local port you like).

```bash
[alia2@localhost .ssh]$ ssh -f -N -L 5800:172.31.45.237:80 remotehost
```

#### 1.8 Like in 1.6, but on localhost using command line utility verify that localhost and port you have specified act like webserver, returning same result as in 1.6.

```bash
[alia2@localhost .ssh]$ curl http://localhost:5800
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.1//EN" "http://www.w3.org/TR/xhtml11/DTD/xhtml11.dtd"><html><head>
<meta http-equiv="content-type" content="text/html; charset=UTF-8">
                <title>Apache HTTP Server Test Page powered by CentOS</title>
                <meta http-equiv="Content-Type" content="text/html; charset=UTF-8">

    <!-- Bootstrap -->
    <link href="/noindex/css/bootstrap.min.css" rel="stylesheet">
    <link rel="stylesheet" href="noindex/css/open-sans.css" type="text/css" />

<style type="text/css"><!--

body {
  font-family: "Open Sans", Helvetica, sans-serif;
  font-weight: 100;
  color: #ccc;
  background: rgba(10, 24, 55, 1);
  font-size: 16px;
}

h2, h3, h4 {
  font-weight: 200;
}

h2 {
  font-size: 28px;
}

.jumbotron {
  margin-bottom: 0;
  color: #333;
  background: rgb(212,212,221); /* Old browsers */
  background: radial-gradient(ellipse at center top, rgba(255,255,255,1) 0%,rgba(174,174,183,1) 100%); /* W3C */
}

.jumbotron h1 {
  font-size: 128px;
  font-weight: 700;
  color: white;
  text-shadow: 0px 2px 0px #abc,
               0px 4px 10px rgba(0,0,0,0.15),
               0px 5px 2px rgba(0,0,0,0.1),
               0px 6px 30px rgba(0,0,0,0.1);
}

.jumbotron p {
  font-size: 28px;
  font-weight: 100;
}

.main {
   background: white;
   color: #234;
   border-top: 1px solid rgba(0,0,0,0.12);
   padding-top: 30px;
   padding-bottom: 40px;
}

.footer {
   border-top: 1px solid rgba(255,255,255,0.2);
   padding-top: 30px;
}

    --></style>
</head>
<body>
  <div class="jumbotron text-center">
    <div class="container">
          <h1>Hello!</h1>
                <p class="lead">You are here because you're probably a DevOps courses member. In that case you should open <a href="https://www.youtube.com/watch?v=dQw4w9WgXcQ"> THIS LINK </a></p>
                </div>
  </div>
</body></html>
```

## Task 2

#### 2.1. Imagine your localhost has been relocated to Havana. Change the time zone on the localhost to Havana and verify the time zone has been changed properly (may be multiple commands).

```bash
[alia2@localhost .ssh]$ timedatectl list-timezones | grep Havana
America/Havana
[alia2@localhost .ssh]$ date
Sat Dec 18 23:18:45 MSK 2021
[alia2@localhost .ssh]$ timedatectl set-timezone America/Havana
==== AUTHENTICATING FOR org.freedesktop.timedate1.set-timezone ===
Authentication is required to set the system timezone.
Authenticating as: alia2
Password:
==== AUTHENTICATION COMPLETE ===
[alia2@localhost .ssh]$ ls -l /etc/localtime
lrwxrwxrwx. 1 root root 36 Dec 18 15:20 /etc/localtime -> ../usr/share/zoneinfo/America/Havana
[alia2@localhost .ssh]$ date
Sat Dec 18 15:20:24 CST 2021
[alia2@localhost .ssh]$  timedatectl status
      Local time: Sat 2021-12-18 15:21:10 CST
  Universal time: Sat 2021-12-18 20:21:10 UTC
        RTC time: Sat 2021-12-18 20:21:10
       Time zone: America/Havana (CST, -0500)
     NTP enabled: n/a
NTP synchronized: no
 RTC in local TZ: no
      DST active: no
 Last DST change: DST ended at
                  Sun 2021-11-07 00:59:59 CDT
                  Sun 2021-11-07 00:00:00 CST
 Next DST change: DST begins (the clock jumps one hour forward) at
                  Sat 2022-03-12 23:59:59 CST
                  Sun 2022-03-13 01:00:00 CDT
```

#### 2.2. Find all systemd journal messages on localhost, that were recorded in the last 50 minutes and originate from a system service started with user id 81 (single command).

```bash
[alia2@localhost .ssh]$ journalctl _UID=81 --since "50 minutes ago"
-- Logs begin at Sat 2021-12-18 11:05:45 CST, end at Sat 2021-12-18 15:21:10 CST. --
Dec 18 15:18:14 localhost.localdomain dbus[672]: [system] Connection has not authenticated soon enough, closing it (auth
Dec 18 15:19:57 localhost.localdomain dbus[672]: [system] Activating via systemd: service name='org.freedesktop.timedate
Dec 18 15:19:57 localhost.localdomain dbus[672]: [system] Successfully activated service 'org.freedesktop.timedate1'
Dec 18 15:21:10 localhost.localdomain dbus[672]: [system] Activating via systemd: service name='org.freedesktop.timedate
Dec 18 15:21:10 localhost.localdomain dbus[672]: [system] Successfully activated service 'org.freedesktop.timedate1'
.
.
.
.
lines 1-6/6 (END)
```

#### 2.3. Configure rsyslogd by adding a rule to the newly created configuration file /etc/rsyslog.d/auth-errors.conf to log all security and authentication messages with the priority alert and higher to the /var/log/auth-errors file. Test the newly added log directive with the logger command (multiple commands).

```bash
[alia2@localhost .ssh]$ cd /var/log/
[alia2@localhost log]$ sudo  touch auth-errors
[alia2@localhost log]$  cd /etc/rsyslog.d/
[alia2@localhost rsyslog.d]$ sudo nano auth-errors.conf
[alia2@localhost rsyslog.d]$ service rsyslog restart
Redirecting to /bin/systemctl restart rsyslog.service
==== AUTHENTICATING FOR org.freedesktop.systemd1.manage-units ===
Authentication is required to manage system services or units.
Authenticating as: alia2
Password:
==== AUTHENTICATION COMPLETE ===
[alia2@localhost rsyslog.d]$ logger -p authpriv.alert log
[alia2@localhost rsyslog.d]$ tail -5 /var/log/auth-errors
Dec 18 15:28:33 localhost alia2: log
```
