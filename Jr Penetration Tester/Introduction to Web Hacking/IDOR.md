IDOR = *Insecure Direct Object Reference*

## Example

Curiosity gets the better of you, and you {*try changing the user_id value to 1000 instead (http://online-service.thm/profile?user_id=1000)*}, and  to your surprise, you can now see another user's information. You've now discovered an IDOR vulnerability! Ideally, there should be a check on  the website to confirm that the user information belongs to the user  logged requesting it.

# Finding IDORs in Encoded IDs

base64 {encode}{decode} are mostly used https://www.base64decode.org/  https://www.base64encode.org/

# Finding IDORs in Hashed IDs

md5 hash are used \https://crackstation.net/

# Finding IDORs in Unpredictable IDs

If the Id cannot be detected using the above methods, an excellent method  of IDOR detection is to create two accounts and swap the Id numbers  between them. If you can view the other users' content using their Id  number while still being logged in with a different account (or not  logged in at all), you've found a valid IDOR vulnerability.

# Where are IDORs located

The vulnerable endpoint you're targeting may not always be something you  see in the address bar. It could be content your browser loads in via an AJAX request or something that you find referenced in a JavaScript  file. 

Sometimes endpoints could have an unreferenced parameter that may have been of  some use during development and got pushed to production. For example,  you may notice a call to **/user/details** displaying your user information (authenticated through your session). But through an attack known as parameter mining, you discover a parameter called **user_id** that you can use to display other users' information, for example, **/user/details?user_id=123**

<img title="" src="https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/5d71d3fe747a8c8934564feddfc69f75.png" alt="img" width="902" data-align="right">