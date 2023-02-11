# Network Utilities

## nmap

nmap is one of the most important tools in a pen testers arsenal. It  allows a pen tester to see which ports are open, and information about  which services are running on those ports. Ergo this task will focus on  showing you nmap's various flags.

- What does nmap stand for?

​       `Network Mapper`                                                                                                     

- How do you specify which port(s) to scan?

  `-p`

- How do you do a "ping scan"(just tests if the host(s) is up)?

  `-sn`

- What is the flag for a UDP scan? 

​       `-sU`                                                                                            

- How do you run default scripts?

​        `-sC`                                                                                            

- How do you enable "aggressive mode"(Enables OS detection, version detection, script scanning, and traceroute)

  `-A`

-  What flag enables OS detection

  `-O`

- How do you get the versions of services running on the target machine  

​       `-sV`

- How many ports are open on the machine?  

​        `1`                                                                                                                                                 

- What service is running on the machine?       

​         `apache`                                                                 

- What is the version of the service?

​        `2.4.18`                                 

- What is the output of the http-title script(included in default scripts)

  `Apache2 Ubuntu Default Page: It works`

##  Netcat                            

Netcat aka nc is an extremely versatile tool. It allows users to  connect to specific ports and send and receive data. It also allows  machines to receive data and connections on specific ports, which makes  nc a very popular tool to gain a [Reverse Shell.](https://resources.infosecinstitute.com/icmp-reverse-shell/#gref)

After you connect to a port with nc you will be able to send data, this also  has the consequence of the user being able to pipe data through nc. For  example one can do` echo hello | nc <ip> 1234` to send the string hello to the service running on port 1234

Note: There are multiple versions of nc, so if you are unable to find an  answer in your specific man page, try reading the man page for others!                                                 

- How do you listen for connections? 

​       `-l`                                                    

- How do you enable verbose mode(allows you to see who connected to you)?

​       `-v`                                                

- How do you specify a port to listen on

​       `-p`                                                  

- How do you specify which program to execute after you connect to a host(One of the most infamous)?

​       `-e`

- How do you connect to udp ports

  `-u`

# Web Enumeration

## gobuster

One of the main problems of web penetration testing is not knowing  where anything is. Basic reconnaissance can tell you where some files  and directories are; however, some of the more hidden stuff is often  hidden away from the eyes of users. This is where gobuster comes in, the idea behind gobuster is that it tries to find valid directories from a  wordlist of possible directories. gobuster can also be used to valid  subdomains using the same method.

- How do you specify directory/file brute forcing mode?

​       `dir`

-  How do you specify dns bruteforcing mode?

​       `dns`

- What flag sets extensions to be used?

  Example: if the php extension is set, and the word is "admin" then gobuster will test admin.php against the webserver

​      `-x`

- What flag sets a wordlist to be used?

​       `-w`

- How do you set the Username for basic authentication(If the directory requires a username/password)?

​       `-U`

- How do you set the password for basic authentication?

​       `-P`

- How do you set which status codes gobuster will interpret as valid? 

  Example: 200,400,404,204

​       `-s`

- How do you skip ssl certificate verification?

​       `-k`

- How do you specify a User-Agent?

​       `-a`

- How do you specify a HTTP header?

​       `-H`

- What flag sets the URL to bruteforce?

​       `-u`

- What is the name of the hidden directory?

​       `secret`

- What is the name of the hidden file with the extension xxa?

​       `password`

## nikto

nikto is a popular web scanning tool that allows users to find common  web vulnerabilities. It is commonly used to check for common CVE's such  as shellshock, and to get general information about the web server that  you're enumerating. 

- How do you specify which host to use

​       `-h`

- What flag disables ssl?

  `-nossl`

- How do you force ssl?

​    `-ssl`

- How do you specify authentication(username + pass)?

​    `-id`

- How do you select which plugin to use?

  `-plugins`

- Which plugin checks if you can enumerate apache users?

​       `Apache Users`

- How do you update the plugin list?

​       `-update`

- How do you list all possible plugins to use?

​       `-list-plugins`

# Metasploit

## Setting Up

Once you have installed metasploit through either the [installer](https://github.com/rapid7/metasploit-framework/wiki/Nightly-Installers) or your distributions repos, you will have many new commands available  to you. This section will primarily focus on the msfconsole command.

- What command allows you to search modules?

  `search`

- How do you select a module?

​       `use`

- How do you display information about a specific module?

​       `info`

- How do you list options that you can set?

  `options`            

- What command lets you view advanced options for a specific module?

  `advanced`

- How do you show options in a specific category

​       `show`

##  Selecting a module

Once you have found the module for the specific machine that you want to exploit, you need to  select it and set the proper options. This task will take you through  selecting and setting options for one of the most popular metasploit  modules "eternalblue". All basic commands that could be run before  selecting a module can also be done while a module is selected. 

- How do you select the eternalblue module?

  `use exploit/windows/smb/ms17_010_eternalblue`

- What option allows you to select the target host(s)?

  `RHOSTS`

- How do you set the target port?

  `RPORT`

-  What command allows you to set options?
   `set`      

- How would you set SMBPass to "username"?

   `set SMBPass username`

- How would you set the SMBUser to "password"

  `set SMBUser password`

- What option sets the architecture to be exploited

  `arch`

- What option sets the payload to be sent to the target machine?

  `payload`

- Once you've finished setting all the required options, how do you run the exploit?

  `exploit`

- What flag do you set if you want the exploit to run in the background?

  `-j`

- How do you list all current sessions?

  `sessions`

- What flag allows you to go into interactive mode with a session("drops you either into a meterpreter or regular shell")

  `-i`

## meterpreter                            

Once you've run the exploit, ideally it will give you one of two  things, a regular command shell or a meterpreter shell. Meterpreter is  metasploits own "control center" where you can do various things to  interact with the machine. A list of common meterpreter commands and  their uses can be found [here](https://www.offensive-security.com/metasploit-unleashed/meterpreter-basics/)

Note: Regular shells can usually be upgraded to meterpreter shells by using the module `post/multi/manage/shell_to_meterpreter`

​                                                            

- What command allows you to download files from the machine?

​       `download`                                               

- What command allows you to upload files to the machine?

​       `upload`                                                 

- How do you list all running processes?

​       `ps`                                                   

- How do you change processes on the  victim host(Ideally it will allow you to change users and gain the perms associated with that user)

​       `migrate`                                                   

- What command lists files in the current directory on the remote machine?
      `ls`                                           

- How do you execute a command on the remote host?

​       `execute`                                                

- What command starts an interactive shell on the remote host?

​       `shell`                                                  

- How do you find files on the target host(Similar function to the linux command "find")

​       `search`                                                  

- How do you get the output of a file on the remote host?

​       `cat`                                     

- How do you put a meterpreter shell  into "background mode"(allows you to run other msf modules while also  keeping the meterpreter shell as a session)?

​       `background`                                                                                                     

   

# Hash Cracking

## Salting and Formatting                            

No matter what tool you use, virtually all of them have the exact  same format. A file with the hash(s) in it with each hash being  separated by a newline.

Example:

<hash 1>

<hash 2>

<hash 3>



[Salt](https://en.wikipedia.org/wiki/Salt_(cryptography))s are typically appended onto the hash with a colon and the salt. Files  with salted hashes still follow the same convention with each hash being separated by a newline.

Example:

<hash1>:<salt>

<hash2>:<salt>

<hash3>:<salt>



Note: Different hashing algorithms treat salts differently. Some prepend them and some append them. Research what it is you're trying to crack, and  make the distinction.

## Hashcat                            

hashcat is another one of the most popular hash cracking tools. It is renowned for its versatility and speed. Hashcat does not have auto  detection for hashtypes, instead it has modes. For example if you were  trying to crack an md5 hash the "mode" would be 0, while if you were  trying to crack a sha1 hash, the mode would be 100.

A full list of all modes can be found [here](https://hashcat.net/wiki/doku.php?id=example_hashes).

- What flag sets the mode.

  `-m`          

- What flag sets the "attack mode"

  `-a`

- What is the attack mode number for Brute-force

  `3`  

```shell
#Start Brute Forcing
hashcat -a 0 -m 100 --session session1 hash.txt pass.txt

#Restore later, if you terminated the brute force
hashcat --restore --session session1
```

## John The Ripper

- What flag let's you specify which wordlist to use?

  `--wordlist` 

- What flag lets you specify which hash format(Ex: MD5,SHA1 etc.) to use? 

  `--format`  

- How do you specify which rule to use?

  `--rules`

```shell
john hash.txt wordlist.txt
```

#  SQL Injection

SQL injection is the art of modifying a SQL query so you can get access  to the target's database. This technique is often used to get user's  data such as passwords, emails etc. SQL injection is one of the most  common web vulnerabilities, and as such, it is highly worth checking for.

## sqlmap

Sqlmap is arguably the most popular automated SQL injection tool out  there. It checks for various types of injections, and has plenty of  customization options.

- How do you specify which url to check?

  `-u`

- What about which google dork to use?

  `-g`

- How do you select(lol) which parameter to use?(Example: in the url [http://ex.com?test=1](http://ex.com?test=1)) the parameter would be test.)

  `-p`

- What flag sets which database is in the  target host's backend?(Example: If the flag is set to mysql then sqlmap  will only test mysql injections).

  `--dbms`

- How do you select the level of depth sqlmap should use(Higher = more accurate and more tests in general).

  `--level`

- How do you dump the table entries of the database?

  `--dump`

- Which flag sets which db to enumerate?

  `-D`

- Which flag sets which table to enumerate?

  `-T`

- Which flag sets which column to enumerate?

  `-C`

- How do you ask sqlmap to try to get an interactive os-shell?

  `--os-shell`

- What flag dumps all data from every table

  `--dump-all`

  ```shell
  sqlmap -u http://<ip> --forms --dump --batch
  ```

  

## A Note on Manual SQL Injection

Occasionally you will be unable to use sqlmap. This can be for a  variety of reasons, such as a the target has set up a firewall or a  request limit. In this case it is worth knowing how to do basic manual  SQL Injection, if only to confirm that there is SQL Injection. A list of ways to check for SQL Injection can be found [here.](https://www.owasp.org/index.php/SQL_Injection)

Note: As there are various ways to check for sql injection, and it would be  difficult to properly convey how to test for sqli given each situation,  there will be no questions for this task.

# Samba

Most of the pentesting techniques and tools you've seen so far can be used on both Windows and Linux. However, one of the things you'll find most often when pen testing  Windows machines is samba, and it is worth making a section dedicated to enumerating it. 

Note: Samba is cross  platform as well, however this section will primarily be focused on  Windows enumeration; some of the techniques you see here still apply to Linux as well.

## smbmap

smbmap is one of the best ways to enumerate samba. smbmap allows  pen-testers to run commands(given proper permissions), download and  upload files, and overall is just incredibly useful for smb enumeration.

- How do you set the username to authenticate with?

  `-u`

- What about the password?

  `-p`

- How do you set the host?

  `-h`

- What flag runs a command on the server(assuming you have permissions that is)?

  `-x`

- How do you specify the share to enumerate?

  `-s`

- How do you set which domain to enumerate?

  `-d`

- What flag downloads a file?

  `--download`

- What about uploading one?

  `--upload`

## smbclient                            

smbclient allows you to do most of the things you can do with smbmap, and it also offers you and interactive prompt.                                                            

- How do you specify which domain(workgroup) to use when connecting to the host?

  `-W`

- How do you specify the ip address of the host? 

  `-I`                       

- How do you run the command "ipconfig" on the target machine?

  `-c  "ipconfig"`

- How do you specify the username to authenticate with?

  `-U`

- How do you specify the password to authenticate with?
- `-P`

- What flag is set to tell smbclient to not use a password?

  `-N`

- While in the interactive prompt, how would you download the file test, assuming it was in the current directory?

  `get`

- In the interactive prompt, how would you upload your /etc/hosts file

  `put /etc/hosts`

## A note about impacket

impacket is a collection of extremely useful windows scripts. It is  worth mentioning here, as it has many scripts available that use samba  to enumerate and even gain shell access to windows machines. All scripts can be found [here](https://github.com/SecureAuthCorp/impacket).

Note: impacket has scripts that use other protocols and services besides samba.

# privilege escalation                            

## General:

https://github.com/swisskyrepo/PayloadsAllTheThings (A bunch of tools and payloads for every stage of pentesting)[
](https://github.com/swisskyrepo/PayloadsAllTheThings)



## Linux:

https://blog.g0tmi1k.com/2011/08/basic-linux-privilege-escalation/ (a bit old but still worth looking at)

https://github.com/rebootuser/LinEnum (One of the most popular priv esc scripts)

https://github.com/diego-treitos/linux-smart-enumeration/blob/master/lse.sh (Another popular script)

https://github.com/mzet-/linux-exploit-suggester (A Script that's dedicated to searching for kernel exploits)



[https://gtfobins.github.io](https://gtfobins.github.io/) (I can not overstate the usefulness of this for priv esc, if a common  binary has special permissions, you can use this site to see how to get  root perms with it.)



## Windows:



https://www.fuzzysecurity.com/tutorials/16.html (Dictates some very useful commands and methods to enumerate the host and gain intel)



https://github.com/PowerShellEmpire/PowerTools/tree/master/PowerUp (A bit old but still an incredibly useful script)



https://github.com/411Hall/JAWS (A general enumeration script)





# Final

## nmap

```shell
Starting Nmap 7.92 ( https://nmap.org ) at 2022-04-19 15:22 IST
Nmap scan report for 10.10.70.179
Host is up (0.15s latency).
Not shown: 998 closed tcp ports (conn-refused)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 12:96:a6:1e:81:73:ae:17:4c:e1:7c:63:78:3c:71:1c (RSA)
|   256 6d:9c:f2:07:11:d2:aa:19:99:90:bb:ec:6b:a1:53:77 (ECDSA)
|_  256 0e:a5:fa:ce:f2:ad:e6:fa:99:f3:92:5f:87:bb:ba:f4 (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 25.01 seconds

OSVDB-3092: /secret/: This might be interesting...
OSVDB-3233: /icons/README: Apache default file found.
http://<ip>/secret/secret.t
```

