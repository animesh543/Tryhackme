#                                            Walking An Application

## Exploring The Website



| **Summary**             |                       |                                                              |
| ----------------------- | --------------------- | ------------------------------------------------------------ |
| Home Page               | /                     | This page contains a summary of what Acme IT Support does with a company photo of their staff. |
| Latest News             | /news                 | This page contains a list of recently published news articles by the  company, and each news article has a link with an id number, i.e.  /news/article?id=1 |
| News Article            | /news/article?id=1    | Displays the individual news article. Some articles seem to be blocked and reserved for premium customers only. |
| Contact Page            | /contact              | This page contains a form for customers to contact the company. It contains  name, email and message input fields and a send button. |
| Customers               | /customers            | This link redirects to /customers/login.                     |
| Customer Login          | /customers/login      | This page contains a login form with username and password fields. |
| Customer Signup         | /customers/signup     | This page contains a user-signup form that consists of a username, email, password and password confirmation input fields. |
| Customer Reset Password | /customers/reset      | Password reset form with an email address input field.       |
| Customer Dashboard      | /customers            | This page contains a list of the user's tickets submitted to the IT support company and a "Create Ticket" button. |
| Create Ticket           | /customers/ticket/new | This page contains a form with a textbox for entering the IT issue and a file upload option to create an IT support ticket. |
| Customer Account        | /customers/account    | This page allows the user to edit their username, email and password. |
| Customer Logout         | /customers/logout     | This link logs the user out of the customer area.            |

## Viewing The Page Source

 At the top of the page, you'll notice some code starting with `<!--` and ending with `-->` these are comments. Comments are messages left by the website  developer, usually to explain something in the code to other programmers or even notes/reminders for themselves. These comments don't get  displayed on the actual webpage. This comment describes how the homepage is temporary while a new one is in development. View the webpage in the comment to get your first flag.

​                            ![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/ccc306acf34d0c83ffd18f33da1a5994.png)

Links to different pages in HTML are written in anchor tags ( these are HTML elements that start with `<a` ), and the link that you'll be directed to is stored in the `href` attribute.

## Developer Tools

### Inspector

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/0e3f5c5c8dd02916d6fc2637461293a9.png)

Locate the `DIV` element with the class `premium-customer-blocker` and click on it. You'll see all the CSS styles in the styles box that apply to this element, such as `margin-top: 60px` and `text-align: center`. The style we're interested in is the `display: block`. If you click on the word `block`, you can type a value of your own choice. Try typing `none`, and this will make the box disappear, revealing the content underneath  it and a flag. If the element didn't have a display field, you could  click below the last style and add in your own. Have a play with the  element inspector, and you'll see you can change any of the information  on the website, including the content. Remember this is only edited on  your browser window, and when you press refresh, everything will be back to normal.

### Debugger

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/d76d0b263799865be55d6725cbeaf21b.png)

### Network

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5efe36fb68daf465530ca761/room-content/9ec0890f159397547950ea1822994443.png)







