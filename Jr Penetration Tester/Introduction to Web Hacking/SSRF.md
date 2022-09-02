**SSRF** ***Server-Side Request Forgery***

The below example shows how the attacker can have complete control over the page requested by the webserver. The Expected Request is what the website.thm server is expecting to receive, with the section in red being the URL that the website will fetch for the information.
The attacker can modify the area in red to an URL of their choice.

![img](https://static-labs.tryhackme.cloud/sites/ssrf-examples/images/ssrf_1.png)

The below example shows how an attacker can still reach the /api/user  page with only having control over the path by utilising directory  traversal. When website.thm receives ../ this is a message to move up a  directory which removes the /stock portion of the request and turns the  final request into /api/user 

![img](https://static-labs.tryhackme.cloud/sites/ssrf-examples/images/ssrf_2.png)

In this example, the attacker can control the server's subdomain to  which the request is made. Take note of the payload ending in **&x=** being used to stop the remaining path from being appended to the end of the attacker's URL and instead turns it into a parameter (?x=) on the  query string

![img](https://static-labs.tryhackme.cloud/sites/ssrf-examples/images/ssrf_3.png)

Going back to the original request, the attacker can instead force the  webserver to request a server of the attacker's choice. By doing so, we  can capture request headers that are sent to the attacker's specified  domain. These headers could contain authentication credentials or API  keys sent by website.thm (that would normally authenticate to  api.website.thm).

![img](https://static-labs.tryhackme.cloud/sites/ssrf-examples/images/ssrf_4.png)

# Finding SSRF

Potential SSRF 
vulnerabilities can be spotted in web applications in many different 
ways. Here is an example of four common places to look:

**When a full URL is used in a parameter in the address bar:**

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/956e1914b116cbc9e564e3bb3d9ab50a.png)  

**A hidden field in a form:**

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/237696fc8e405d25d4fc7bbcc67919f0.png)  

**A partial URL such as just the hostname:**

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/f3c387849e91a4f15a7b59ff7324be75.png)

**Or perhaps only the path of the URL:**

![](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/3fd583950617f7a3713a107fcb4cfa49.png)

Some
 of these examples are easier to exploit than others, and this is where a
 lot of trial and error will be required to find a working payload.

If working with a blind SSRF where no output is reflected back to you, you'll need to use an external HTTP logging tool to monitor requests such as requestbin.com, your own HTTP server or Burp Suite's Collaborator client.
