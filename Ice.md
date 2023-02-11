# Recon

## namp

```shell
sudo nmap -sS -sC -sV -p- -T4 10.10.84.251
Starting Nmap 7.92 ( https://nmap.org ) at 2022-05-21 16:58 IST
Nmap scan report for 10.10.84.251
Host is up (0.17s latency).
Not shown: 65523 closed tcp ports (reset)
PORT      STATE SERVICE            VERSION
135/tcp   open  msrpc              Microsoft Windows RPC
139/tcp   open  netbios-ssn        Microsoft Windows netbios-ssn
445/tcp   open  microsoft-ds       Windows 7 Professional 7601 Service Pack 1 microsoft-ds (workgroup: WORKGROUP)
3389/tcp  open  ssl/ms-wbt-server?
|_ssl-date: 2022-05-21T12:03:29+00:00; +17m11s from scanner time.
| rdp-ntlm-info: 
|   Target_Name: DARK-PC
|   NetBIOS_Domain_Name: DARK-PC
|   NetBIOS_Computer_Name: DARK-PC
|   DNS_Domain_Name: Dark-PC
|   DNS_Computer_Name: Dark-PC
|   Product_Version: 6.1.7601
|_  System_Time: 2022-05-21T12:03:25+00:00
| ssl-cert: Subject: commonName=Dark-PC
| Not valid before: 2022-05-20T11:08:39
|_Not valid after:  2022-11-19T11:08:39
5357/tcp  open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Service Unavailable
8000/tcp  open  http               Icecast streaming media server
|_http-title: Site doesn't have a title (text/html).
49152/tcp open  msrpc              Microsoft Windows RPC
49153/tcp open  msrpc              Microsoft Windows RPC
49154/tcp open  msrpc              Microsoft Windows RPC
49159/tcp open  msrpc              Microsoft Windows RPC
49160/tcp open  msrpc              Microsoft Windows RPC
49161/tcp open  msrpc              Microsoft Windows RPC
Service Info: Host: DARK-PC; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2022-05-21T12:03:24
|_  start_date: 2022-05-21T11:08:38
|_nbstat: NetBIOS name: DARK-PC, NetBIOS user: <unknown>, NetBIOS MAC: 02:fb:f1:e5:a2:4b (unknown)
| smb-os-discovery: 
|   OS: Windows 7 Professional 7601 Service Pack 1 (Windows 7 Professional 6.1)
|   OS CPE: cpe:/o:microsoft:windows_7::sp1:professional
|   Computer name: Dark-PC
|   NetBIOS computer name: DARK-PC\x00
|   Workgroup: WORKGROUP\x00
|_  System time: 2022-05-21T07:03:24-05:00
| smb2-security-mode: 
|   2.1: 
|_    Message signing enabled but not required
| smb-security-mode: 
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
|_clock-skew: mean: 1h17m11s, deviation: 2h14m10s, median: 17m11s
```

- Once the scan completes, we'll see a  number of interesting ports open on this machine. As you might have  guessed, the firewall has been disabled (with the service completely  shutdown), leaving very little to protect this machine. One of the more  interesting ports that is open is Microsoft Remote Desktop (MSRDP). What port is this open on?

  `3389`

- What service did nmap identify as running on port 8000? (First word of this service)

  `icecast`

- What does Nmap identify as the hostname of the machine? (All caps for the answer)

  `DARK-PC`



# Gain Access

This task was mainly concerned with exploiting vulnerability and gaining access to system.

\#1 Browsed to the website https://www.cvedetails.com and searched for Icecast vulnerabilities and looked for high rating  issues with 7.5 rating. Type of vulnerability for this issue was marked  as “execute code overflow”.

![img](https://stawmdesign.files.wordpress.com/2020/05/cve.png?w=1024)

\#2 The identifier of the vulnerability was CVE-2004-1561 (please refer image in the last subtask).

\#3 No answer needed for this subtask. This subtask was mainly  concerned with starting Metasploit Framework using msfconsole command.

\#4 The full path to Icecast exploit module is “exploit/windows/http/icecast_header”.

![img](https://stawmdesign.files.wordpress.com/2020/05/icecsat-msf-cmodule.png?w=1024)

\#5 No answer needed for this subtask. Just need to use the exploit module identified in last subtask.

![img](https://stawmdesign.files.wordpress.com/2020/05/set-rhosts.png?w=1024)

\#6 After selecting the module, type options (or show options) and you will get module options list. From the list only required missing  setting is RHOSTS.  

![img](https://stawmdesign.files.wordpress.com/2020/05/set-rhosts-1.png?w=1024)

\#7 No answer needed for this subtask. Configure the setting of RHOSTS to the target IP of “DARK-PC”. Execute the module using exploit command and you will get a shell to target system.

# Escalate

This task is mainly concerned with privilege escalation to gain system access on target system.

\#1 Meterpreter is the shell we got because of successful exploitation.

![img](https://stawmdesign.files.wordpress.com/2020/05/shell.png?w=1024)

\#2 “Dark” is the user running Icecast process.

![img](https://stawmdesign.files.wordpress.com/2020/05/user-running-icecast.png?w=365)

\#3 Windows build is 7601.

![img](https://stawmdesign.files.wordpress.com/2020/05/windows-build.png?w=652)

\#4 Architecture is x64.

\#5 No answer needed. This subtask requires you to run post  exploitation reconnaissance (run  post/multi/recon/local_exploit_suggester)

![img](https://stawmdesign.files.wordpress.com/2020/05/local-exploit-suggester.png?w=1024)

\#6 The first suggested exploit was “exploit/windows/local/bypassuac_eventvwr”.

\#7 No answer needed. This subtask requires you to background current session by pressing CTRL+Z.

![img](https://stawmdesign.files.wordpress.com/2020/05/backgrounded-session.png?w=1024)

\#8 No answer needed. This subtask requires you to input following command on msfconsole:

use exploit/windows/local/bypassuac_eventvwr

\#9 No answer needed. This subtask requires you “use command set session 1”, because our backgrounded session had id 1.

\#10 “LHOST”is the option that needs to be set.

![img](https://stawmdesign.files.wordpress.com/2020/05/all-options-set.png?w=1024)

\#11 No answer needed. Your attack system IP (IP of tun0) can be  checked using ifconfig tun0 or from msfconsole using ip addr command.

\#12 No answer needed.

\#13 No answer needed.

\#14 “SeTakeOwnershipPrivilege” is the privilege that allows us to take ownership of files.

![img](https://stawmdesign.files.wordpress.com/2020/05/getprivs.png?w=1024)

# Looting

This task is concerned mainly with gathering and cracking credentials.

\#1 No answer needed. This subtask requires you to list processes on target system.

![img](https://stawmdesign.files.wordpress.com/2020/05/ps-output.png?w=1024)

\#2 The printer service is “spoolsv.exe” with x64 architecture.

![img](https://stawmdesign.files.wordpress.com/2020/05/printer-service-with-nt-authroity.png?w=1024)

\#3 No answer needed. This subtask requires you to migrate our process to “spoolsv.exe”.

![img](https://stawmdesign.files.wordpress.com/2020/05/migration-to-spollsv.png?w=558)

\#4 After migration to “spoolsv.exe” we have “NT AUTHORITY\SYSTEM” user.

![img](https://stawmdesign.files.wordpress.com/2020/05/migration-nt-system.png?w=577)

\#5 No answer needed. This subtask requires you to Mimikatz module using load kiwi command.

![img](https://stawmdesign.files.wordpress.com/2020/05/kiwiw-loaod.png?w=696)

\#6 No answer needed. This subtask requires you to view help using command help and explore kiwi module commands.

![img](https://stawmdesign.files.wordpress.com/2020/05/kiwi-help-added.png?w=1024)

\#7 “creds_all” is the command that lets you retrieve all credentials.

![img](https://stawmdesign.files.wordpress.com/2020/05/cedsall.png?w=1024)

\#8 Dark’s password is found by running “creds_all” command.

![img](https://stawmdesign.files.wordpress.com/2020/05/credsall.png?w=1024)

# Post-Exploitation

This task wants you to explore post-exploitation actions that can be performed on Windows.

\#1 No answer needed. This subtask requires you to view help using command help and see available commands.

\#2 “hashdump“ allows us to dump all password hashes stored on the system.

![img](https://stawmdesign.files.wordpress.com/2020/05/hashdump-command.png?w=1024)

\#3 “screenshare”command allows us to watch the remote user’s desktop in real time.

![img](https://stawmdesign.files.wordpress.com/2020/05/watch-desktop-help.png?w=1024)

\#4 record_mic.   to record from a microphone attached to the system

![img](https://stawmdesign.files.wordpress.com/2020/05/recordmic.png?w=1024)

\#5 timestomp.                                                              

To complicate forensics efforts we can modify timestamps of files on the system

![img](https://stawmdesign.files.wordpress.com/2020/05/timestomp.png?w=1024)

\#6 golden_ticket_create.   to create what's called a `golden ticket`, allowing us to authenticate anywhere with ease

![img](https://stawmdesign.files.wordpress.com/2020/05/goldernticket-help.png?w=1024)

\#7 No answer needed. For this subtask you need to connect to target machine using remote desktop. 

![img](https://stawmdesign.files.wordpress.com/2020/05/rdp-commadline.png?w=1024)

![img](https://stawmdesign.files.wordpress.com/2020/05/rdp.png?w=1024)

![img](https://stawmdesign.files.wordpress.com/2020/05/rdp-successful.png?w=1024)

![img](https://stawmdesign.files.wordpress.com/2020/05/dark-rdp-access.png?w=1024)