# Introduction                            

There are quite a lot  of vulnerabilities that affect web applications. This leads to the  problem of having quite a bit of material to cover, so it's fair to only cover the bigger/more common vulnerabilities in lieu of the  smaller/less common vulnerabilities. This room is dedicated to some of  the smaller web vulnerabilities, that may not be large enough to get a  room on this site but are still worth knowing about. 

The vulnerabilities that will be discussed are:

SSTI
CSRF
JWT
XXE

# What is SSTI



A template engine allows developers to use static HTML pages with  dynamic elements. Take for instance a static profile.html page, a  template engine would allow a developer to set a username parameter,  that would always be set to the current user's username

Server Side Template Injection, is when a user is able to pass in a parameter  that can control the template engine that is running on the server.

For example take the code  

![img](https://imgur.com/GAO1km1.png)

This introduces a vulnerability, as it allows a hacker to inject template  code into the website. The effects of this can be devastating, from XSS, all the way to RCE.

Note:  Different template engines have different injection payloads, however  usually **you can test for SSTI using `{{2+2}}` as a test.**

## Manual exploitation of SSTI.                            

Turning the code earlier into a full flask application, gives us this page. It takes a prompt for a name, and then returns `Hello <name>!`. 

![img](https://imgur.com/j0FJBw5.png)

Fortunately, we don't have to do much recon as we already know this is vulnerable to SSTI, lets try injecting some basic template code

![img](https://imgur.com/CdzYeHS.png)

Boom! That's template injection. We can use the wonderful repository [PayloadsAllTheThings](https://github.com/swisskyrepo/PayloadsAllTheThings/tree/master/Server Side Template Injection#basic-injection), to find some payloads for flask's template engine. The repo says we can use the code 

- **`{{ ''.__class__.__mro__[2].__subclasses__()[40]()(<file>).read()}} ` to read files on the server. Effectively all that payload does is load the file object in python, from there we can use basic file operations.  Let's try to read /etc/passwd using this method** 

![img](https://imgur.com/a7RphqV.png)

We have LFI! Unfortunately(or fortunately depending on how you view it),  that is not the extent of this vulnerability. The same repo, also  includes a payload for remote code execution. 

- **We can use the code `{{config.__class__.__init__.__globals__['os'].popen(<command>).read()}}` to execute commands on the server. All that payload does is import the os module, and run a command using the popen method.**

![img](https://imgur.com/XTDPJSX.png)

From there, an attacker has already won, they can use this ability to gain a shell on the server.

##  Automatic Exploitation of SSTI                            

Fortunately, we don't  have to go searching for payloads to see how we can use SSTI to our  advantage, because there is a tool known as Tplmap that does that for  us! The tool can be found [here](https://github.com/epinna/tplmap). 

Note: use python2 to install the requirements. python2 -m pip

The basic syntax for tplmap is different depending on whether you're using get or post

| GET  | tplmap -u <url>/?<vulnparam>     |
| ---- | -------------------------------- |
| POST | tplmap -u <url> -d '<vulnparam>' |

Since our code operates via a form, the post syntax will be used.

![img](https://imgur.com/416wOwX.png)

From there we can effectively do everything we did in the manual  exploitation task, from a command line. Let's try running id using  tplmap.

![img](https://imgur.com/eqUQETT.png)

Victory!

- How would I cat out /etc/passwd using tplmap on the ip:port combo 10.10.10.10:5000, with the vulnerable param "noot"  `tplmap -u http://10.10.10.10:5000/ -d 'noot' --os-cmd "id" /etc/passwd`

### Challenge

`{{config.__class__.__init__.__globals__['os'].popen('cat /flag').read()}}`

#  What is CSRF

Cross Site Request Forgery, known as CSRF occurs when a user visits a page on a site, that performs an action on a different site. For  instance, let's say a user clicks a link to a website created by a  hacker, on the website would be an html tag such as `<img src="https://vulnerable-website.com/email/change?email=pwned@evil-user.net"> ` which would change the account email on the vulnerable website to  "pwned@evil-user.net". CSRF works because it's the victim making the  request not the site, so all the site sees is a normal user making a  normal request.

This opens the  door, to the user's account being fully compromised through the use of a password reset for example. The severity of this cannot be overstated,  as it allows an attacker to potentially gain personal information about a user, such as credit card details in an extreme case.

## Manual exploitation of CSRF                            

Let's take an example application 

![img](https://imgur.com/P6oMiiZ.png)

It seems simple enough, As user bob, I can send funds to either Bob or  Alice with any of the available balance in my account. Let's take a  closer look at the request in burp.

![img](https://imgur.com/RYEnGqy.png)

This is looking good, parameters we can customize and a session cookie that  is automatically set. Everything seems vulnerable to CSRF. Let's try and make a vulnerable site. Putting `<img src="http://localhost:3000/transfer?to=alice&amount=100">` into an html file and using SimpleHTTPServer to host it should change's Alice's balance by 100, Let's see if it does!

![img](https://imgur.com/wUUlax4.png)

Woohoo, CSRF exploited!

## Automatic Explotation

Once again, there is a  nice automated scanner, which tests if a site is vulnerable to CSRF.  this tool is known as xsrfprobe and can be install via pip using `pip3 install xsrfprobe`. This will only work using python 3(I mean come on it's 2020 you should be using python 3 anyway).

The syntax for the command is `xsrfprobe -u <url>/<endpoint>`. Let's run this against our vulnerable site.

![img](https://imgur.com/5gx7k8D.png)

The output confirms that we've managed to manually exploiting it and that the site is vulnerable to csrf. 

# Json Web Token

Json Web Token's are a  fairly interesting case, as it isn't a vulnerability itself. Infact,  it's a fairly popular, and if done right very secure method of  authentication. The basic structure of a JWT is this, it goes  "header.payload.secret", the secret is only known to the server, and is  used to make sure that data wasn't changed along the way. Everything is  then base64 encoded. so an example JWT token would look like `"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c"`

Meaning that if we are able to control the secret, we can effectively control  the data. To be able to do this we have to understand how the secret is  calculated. This requires knowing the structure of the header, a typical JWT header looks like this {"typ":"JWT","alg":"RS256"}. We're interested in the alg field. RS256 uses a private RSA key that's only  available to the server, so that's not vulnerable. However, We can  change that field to HS256, This is calculated using the server's *public* key, which in certain circumstances we may have access too.