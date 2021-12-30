# Homework

### Awk /ɔːk/

## ex1

1. What is the most frequent browser?

```bash
[alia2@localhost ~]$ cat access.log | grep -Po '\"\s\".*\"$' | grep -Po '[^\"]{1,}' | awk -F '"' '{arr[$1]+=1}END{for (ip in arr) print arr[ip], ip}' | uniq |sort -k1n | tail -n3 | head -n1
340874 Mozilla/4.0 (compatible; MSIE 7.0; Windows NT 6.0)
```

## ex2

2. Show number of requests per month for ip 216.244.66.230 (for example: Sep 2016 - 100500 reqs, Oct 2016 - 0 reqs, Nov 2016 - 2 reqs...)

```bash
[alia2@localhost ~]$ awk 'BEGIN{cnt=0;}{split($4,y,"/");split(y[3],yy,":");if($1=="216.244.66.230"){r[sprintf("%s %s",y[2],yy[1])]+=1;cnt+=1;}}END{for (i in r) print i, r[i]; print "Total requests:",cnt}' access.log | sort -k1,1M | sort -k2n
Total requests: 394
Dec 2020 1
Apr 2021 34
Aug 2021 108
Dec 2021 3
Feb 2021 14
Jan 2021 43
Jul 2021 23
Jun 2021 3
Mar 2021 2
May 2021 27
Nov 2021 106
Oct 2021 23
Sep 2021 7
```

## ex3

Show total amount of data which server has provided for each unique ip (i.e. 100500 bytes for 1.2.3.4; 9001 bytes for 5.4.3.2 and so on)

```bash
[alia2@localhost ~]$ awk 'BEGIN{ips[$1]=0}{ips[$1]+=$10}END{for (key in ips) print ips[key]" bytes for "key}' access.log
10439 bytes for 34.223.3.249
10439 bytes for 3.19.243.250
.
.
.
10466 bytes for 94.154.220.93
26209910 bytes for 145.253.118.26
```

### Sed

## ex1

1. Change all browsers to "lynx"

```bash
[alia2@localhost ~]$ sed 's/\"[^\"]*\"/\"lynx\"/3' access.log | head -n7

13.66.139.0 - - [19/Dec/2020:13:57:26 +0100] "GET /index.php?option=com_phocagallery&view=category&id=1:almhuette-raith&Itemid=53 HTTP/1.1" 200 32653 "-" "lynx" "-"
157.48.153.185 - - [19/Dec/2020:14:08:06 +0100] "GET /apache-log/access.log HTTP/1.1" 200 233 "-" "lynx" "-"
157.48.153.185 - - [19/Dec/2020:14:08:08 +0100] "GET /favicon.ico HTTP/1.1" 404 217 "http://www.almhuette-raith.at/apache-log/access.log" "lynx" "-"
216.244.66.230 - - [19/Dec/2020:14:14:26 +0100] "GET /robots.txt HTTP/1.1" 200 304 "-" "lynx" "-"
54.36.148.92 - - [19/Dec/2020:14:16:44 +0100] "GET /index.php?option=com_phocagallery&view=category&id=2%3Awinterfotos&Itemid=53 HTTP/1.1" 200 30662 "-" "lynx" "-"
92.101.35.224 - - [19/Dec/2020:14:29:21 +0100] "GET /administrator/index.php HTTP/1.1" 200 4263 "" "lynx" "-"
```

## ex2

2. Masquerade all ip addresses. Rewrite file.

```bash
[alia2@localhost ~]$ sed -r 's/(\b[0-9]{1,3}\.){3}[0-9]{1,3}\b'/"ip"/ access.log | head -n5

ip - - [19/Dec/2020:13:57:26 +0100] "GET /index.php?option=com_phocagallery&view=category&id=1:almhuette-raith&Itemid=53 HTTP/1.1" 200 32653 "-" "Mozilla/5.0 (compatible; bingbot/2.0; +http://www.bing.com/bingbot.htm)" "-"
ip - - [19/Dec/2020:14:08:06 +0100] "GET /apache-log/access.log HTTP/1.1" 200 233 "-" "Mozilla/5.0 (Windows NT 6.3; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36" "-"
ip - - [19/Dec/2020:14:08:08 +0100] "GET /favicon.ico HTTP/1.1" 404 217 "http://www.almhuette-raith.at/apache-log/access.log" "Mozilla/5.0 (Windows NT 6.3; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/87.0.4280.88 Safari/537.36" "-"
ip - - [19/Dec/2020:14:14:26 +0100] "GET /robots.txt HTTP/1.1" 200 304 "-" "Mozilla/5.0 (compatible; DotBot/1.1; http://www.opensiteexplorer.org/dotbot, help@moz.com)" "-"
```
