# **What is HTTP? (HyperText Transfer Protocol)**

HTTP is what's used whenever you view a website, developed by Tim  Berners-Lee and his team between 1989-1991. HTTP is the set of rules  used for communicating with web servers for the transmitting of webpage  data, whether that is HTML, Images, Videos, etc.

# **What is HTTPS?**\***(HyperText Transfer Protocol Secure)***

HTTPS is the secure version of HTTP.  HTTPS data is encrypted so it not only stops people from seeing the data you are receiving and sending, but it also gives you assurances that  you're talking to the correct web server and not something impersonating it.

#  Requests And Responses

**What is a URL? (Uniform Resource Locator)**



If you’ve used the internet, you’ve used a URL before. A URL is  predominantly an instruction on how to access a resource on the  internet. The below image shows what a URL looks like with all of its  features (it does not use all features in every request).

![img](https://static-labs.tryhackme.cloud/sites/howhttpworks/newurl.png)

**Scheme:** This instructs on what protocol to use for accessing the resource such as HTTP, HTTPS, FTP (File Transfer Protocol).

**User:** Some services require authentication to log in, you can put a username and password into the URL to log in.

**Host:** The domain name or IP address of the server you wish to access.

**Port:** The Port that you are going to connect to, usually 80 for HTTP and 443  for HTTPS, but this can be hosted on any port between 1 - 65535.

**Path:** The file name or location of the resource you are trying to access.

**Query String:** Extra bits of information that can be sent to the requested path. For example, /blog?**id=1** would tell the blog path that you wish to receive the blog article with the id of 1.

**Fragment:** This is a reference to a location on the actual page requested. This is  commonly used for pages with long content and can have a certain part of the page directly linked to it, so it is viewable to the user as soon  as they access the page. 

**Making a Request**

It's possible to make a request to a web server with just one line "**GET / HTTP/1.1**"

![img](https://static-labs.tryhackme.cloud/sites/howhttpworks/line.png)



But for a much richer web experience, you’ll need to send other data as  well. This other data is sent in what is called headers, where headers  contain extra information to give to the web server you’re communicating with, but we’ll go more into this in the Header task.

# HTTP Methods

## **GET Request**

This is used for getting information from a web server.

## **POST Request**

This is used for submitting data to the web server and potentially creating new records

## **PUT Request**

This is used for submitting data to a web server to update information

## **DELETE Request**

This is used for deleting information/records from a web server.

# HTTP Status Codes

## **HTTP Status Codes:**

| **100-199 - Information Response** | These are sent to tell the client the first part of their request has been  accepted and they should continue sending the rest of their request.  These codes are no longer very common. |
| ---------------------------------- | ------------------------------------------------------------ |
| **200-299 - Success**              | This range of status codes is used to tell the client their request was successful. |
| **300-399 - Redirection**          | These are used to redirect the client's request to another resource. This can be either to a different webpage or a different website altogether. |
| **400-499 - Client Errors**        | Used to inform the client that there was an error with their request. |
| **500-599 - Server Errors**        | This is reserved for errors happening on the server-side and usually  indicate quite a major problem with the server handling the request. |

## **Common HTTP Status Codes:**

| **200 - OK**                     | The request was completed successfully.                      |
| -------------------------------- | ------------------------------------------------------------ |
| **201 - Created**                | A resource has been created (for example a new user or new blog post). |
| **301 - Permanent Redirect**     | This redirects the client's browser to a new webpage or tells search engines that the page has moved somewhere else and to look there instead. |
| **302 - Temporary Redirect**     | Similar to the above permanent redirect, but as the name suggests, this is only a temporary change and it may change again in the near future. |
| **400 - Bad Request**            | This tells the browser that something was either wrong or missing in their  request. This could sometimes be used if the web server resource that is being requested expected a certain parameter that the client didn't  send. |
| **401 - Not Authorised**         | You are not  currently allowed to view this resource until you have authorised with  the web application, most commonly with a username and password. |
| **403 - Forbidden**              | You do not have permission to view this resource whether you are logged in or not. |
| **405 - Method Not Allowed**     | The resource does not allow this method request, for example, you send a  GET request to the resource /create-account when it was expecting a POST request instead. |
| **404 - Page Not Found**         | The page/resource you requested does not exist.              |
| **500 - Internal Service Error** | The server has encountered some kind of error with your request that it doesn't know how to handle properly. |
| **503 - Service Unavailable**    | This server cannot handle your request as it's either overloaded or down for maintenance. |

# Headers

## **Host** 

Some web servers host multiple websites so by providing the host headers you can tell it which one you require, otherwise you'll  just receive the default website for the server.

## **User-Agent** 

This is your browser software and version number,  telling the web server your browser software helps it format the website properly for your browser and also some elements of HTML, JavaScript  and CSS are only available in certain browsers.

## **Content-Length**

 When sending data to a web server such as in a  form, the content length tells the web server how much data to expect in the web request. This way the server can ensure it isn't missing any  data.

**Accept-Encoding**

 Tells the  web server what types of compression methods the browser supports so the data can be made smaller for transmitting over the internet.

**Cookie**

 Data sent to the server to help remember your information (see cookies task for more information).

**Set-Cookie:** Information to store which gets sent back to the web server on each request (see cookies task for more information).

**Cache-Control:** How long to store the content of the response in the browser's cache before it requests it again.

**Content-Type:** This tells the client what type of data is being returned, i.e., HTML,  CSS, JavaScript, Images, PDF, Video, etc. Using the content-type header  the browser then knows how to process the data.

**Content-Encoding:** What method has been used to compress the data to make it smaller when sending it over the internet

# Cookies

![img](https://static-labs.tryhackme.cloud/sites/howhttpworks/cookie_flow.png)