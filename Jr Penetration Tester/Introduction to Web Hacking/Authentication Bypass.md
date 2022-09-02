# Username Enumeration

```shell
ffuf -w /usr/share/wordlists/seclists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://<ip/location to login> -mr "username already exists" 
```

In the above example, the `-w` argument selects the file's location on the computer that contains the list of usernames that we're going to check exists. The `-X` argument specifies the request method, this will be a GET request by default, but it is a POST request in our example. The `-d` argument specifies the data that we are going to send. In our example,  we have the fields username, email, password and cpassword. We've set  the value of the username to **FUZZ**. In the ffuf tool, the FUZZ keyword signifies where the contents from our wordlist will be inserted in the request. The `-H` argument is used for adding additional headers to the request. In this instance, we're setting the `Content-Type` to the webserver knows we are sending form data. The `-u` argument specifies the URL we are making the request to, and finally, the `-mr` argument is the text on the page we are looking for to validate we've found a valid username.

# Brute Force

```shell-session
ffuf -w valid_usernames.txt:W1,/usr/share/wordlists/Seclists/Passwords/Common-Credentials/10-million-password-list-top-100.txt:W2 -X POST -d "username=W1&password=W2" -H "Content-Type: application/x-www-form-urlencoded" -u http://MACHINE_IP/customers/login -fc 200
```

This ffuf command is a little different to the previous one in Task 2. Previously we used the **FUZZ** keyword to select where in the request the data from the wordlists  would be inserted, but because we're using multiple wordlists, we have  to specify our own FUZZ keyword. In this instance, we've chosen `W1` for our list of valid usernames and `W2` for the list of passwords we will try. The multiple wordlists are again specified with the `-w` argument but separated with a comma. For a positive match, we're using the `-fc` argument to check for an HTTP status code other than 200.

Running the above command will find a single working username and password combination that answers the question below.

# Logic Flaw

```shell-session
curl 'http://10.10.227.27/customers/reset?email=robert%40acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert'
```

We use the `-H` flag to add an additional header to the request. In this instance, we are setting the `Content-Type` to `application/x-www-form-urlencoded`, which lets the web server know we are sending form data so it properly understands our request. 

In the application, the user account is retrieved using the query string,  but later on, in the application logic, the password reset email is sent using the data found in the PHP variable `$_REQUEST`. 

The PHP `$_REQUEST` variable is an array that contains data received from the query string  and POST data. If the same key name is used for both the query string  and POST data, the application logic for this variable favours POST data fields rather than the query string, so if we add another parameter to  the POST form, we can control where the password reset email gets  delivered.

```shell-session
curl 'http://10.10.227.27/customers/reset?email=robert@acmeitsupport.thm' -H 'Content-Type: application/x-www-form-urlencoded' -d 'username=robert&email={username}@customer.acmeitsupport.thm'
```

# Cookie Tampering

```shell-session
curl http://MACHINE_IP/cookie-test
```

```shell-session
curl -H "Cookie: logged_in=true; admin=false" http://MACHINE_IP/cookie-test
```

```shell-session
curl -H "Cookie: logged_in=true; admin=true" http://MACHINE_IP/cookie-test
```
# Re-registration
Let's understand this with the help of an example, say there is an existing user with the name **admin** and now we want to get access to their account so what we can do is try to re-register that username but with slight modification. We are going to enter " admin"(notice the space in the starting). Now when you enter that in the username field and enter other required information like email id or password and submit that data. It will actually register a new user but that user will have the same right as normal admin. And that new user will also be able to see all the content present under the user **admin**.
# JSON Web Token
JSON Web Token(JWT) is one of the commonly used methods for authorization. This is a kind of cookie that is generated using HMAC hashing or public/private keys. So unlike any other kind of cookie, it lets the website know what kind of access the currently logged in user has. The only special thing about JWT is that they are in JSON format(after decoding).

JWT can be divided into 3 parts separated by a dot(.)  

1) **Header:** This consists of the algorithm used and the type of the token.

`{  "alg": "HS256", "typ": "JWT"}`

alg could be HMAC, RSA, SHA256 or can even contain None value.

2) **Payload:** This is part that contains the access given to the certain user etc. This can vary from website to website, some can just have a simple username and some ID and others could have a lot of other details.

3) **Signature:** This is the part that is used to make sure that the integrity of the data was maintained while transferring it from a user's computer to the server and back. This is encrypted with whatever algorithm or **alg** that was passed in the header's value. And this can only be decrypted with a predefined secret(which should be difficult to)

Now to put all the 3 part together we base64 encode all of them separated by a dot(.) so it would look something like:

`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c`  

**Note:** This example was taken from [jwt.io](https://jwt.io/#debugger-io) and you should check that website out if you want to learn more about JWT. 

**Exploitation** 

If used properly this is a very secure way of authorization but the problem is with using is "properly". A lot of developers misconfigure their system leaving it open to exploitation.

Now one of the methods to exploit this is to perform a brute force/dictionary attack and find the secret used for encrypting the JWT token and then used that to generate new tokens. But here we are not going to do that, we are going to see a very amazing way of exploiting this.

If you remember, in the Header section I said that the **alg** can be whatever the algorithm is used and also it can be **None** if no encryption is to be used. Now, this should not be used when the application is in production but again the problem of misconfiguration comes in and make the application vulnerable to this kind of attack. The attack is that an attacker can log in as low privilege user says **guest** and then get the JWT token for that user and then decode the token and edit the headers to use set **alg** value to **None**. This would mean that no encryption has to be used therefore the attacker wouldn't need to the secret used for encryption.  


**Practical**

Let's see this method in practice. For this challenge visit the port 5000.

 It is a very simple login page and in that, you can log in via two users: user and user2. Now first let's try to login with the credentials of **user:user** . To do so first enter those credentials then click on the **Authenticate** button and then enable the capture in burp suite and then click on **the Go** button. In the burp tab, you should see a request to **/protected** ﻿and there you'll see the JWT token.


https://imgur.com/sLKHY9

![](https://imgur.com/sLKHY9V.png)

Now take this JWT token and then you can decode it part by part.

So if we decode the first part, which will do: `{"typ":"JWT","alg":"HS256"}`

and decoding the 2nd part, we will get: `{"exp":1586620929,"iat":1586620629,"nbf":1586620629,"identity":1}`

If you try to decode the 3rd part then you'll get some gibberish. But that is okay we only need the first and the second part.

Now if we notice the **identity** value that is probably being used to identify the user but if you'll just edit that then it won't work because as I said the 3rd part is encrypted. So to bypass this we will make changes in the header as well as the value of the identity.

Encode the following string with base64 and that will be our first part

`{"typ":"JWT","alg":"NONE"}`

For the second part, we'll encode the following string:

`{"exp":1586620929,"iat":1586620629,"nbf":1586620629,"identity":2}`

Notice how we changed the value of **identity** from **1** to **2**.

Since we placed the alg value to None we don't have to add a 3rd part or the encrypted value so we can just put a dot(.) after 2nd part and leave it like that. So the final string would look like:

`eyJ0eXAiOiJKV1QiLCJhbGciOiJOT05FIn0K.eyJleHAiOjE1ODY3MDUyOTUsImlhdCI6MTU4NjcwNDk5NSwibmJmIjoxNTg2NzA0OTk1LCJpZGVudGl0eSI6MH0K.  
`

Now open the developer's tools in your browser and edit the stored cookie of the website to this new one and then just press the **Go** button and you'll notice that it will prompt "Welcome user2: guest2".
In a similar manner, you can try to play and find other users on the website.
This kind of misconfiguration in the authentication system is common and could be exploited to escalate privileges or steal information.
# No Auth
In this I am going to show you how a lot of systems don't even have proper authentication and their system is just left open for anyone to exploit it.

A lot of time on websites we see that when we register a user and login with our credentials we are given a certain id which either is completely a number or ends with a number. Most of the time developers secures their application but sometime in some places, it could happen that just by changing that number we are able to see some hidden or private data.

To test this go to port **7777**. On that just create an account. Once the account is created visit your **Private Space**.

![](https://imgur.com/RzRzvpx.png)

As you can see in the image above the URL have **/users/1**. Try to change that value to 2 and we will get access to the admin account  

![](https://imgur.com/TAkXZ4b.png)