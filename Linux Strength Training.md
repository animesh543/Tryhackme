# Finding your way around linux - overview

As a security researcher you will often be required to find specific  files/folders on a system based on various conditions ranging from, but  not limited to the following:

- **filename** 
- **size 
  **
- **user/group**
- **date modified**
- **date accessed
  **
- **Its keyword contents**

Therefore, we can do this using the following syntax:



| What we can do                            | Syntax                                                       | Real example of syntax                                       |
| ----------------------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Find files based on filename              | find [directory path] -type f -name [filename]               | find /home/Andy -type f -name sales.txt                      |
| Find Directory based on directory name    | find [directory path] -type d -name [filename]               | find /home/Andy -type d -name pictures                       |
| Find files based on size                  | find [directory path] -type f -size [size]                   | find /home/Andy -type f -size 10c(c for bytes,k for kilobytesM megabytesG for gigabytestype:'man find' for full information on the options) |
| Find files based on username              | find [directory path] -type f -user [username]               | find /etc/server -type f -user john                          |
| Find files based on group name            | find [directory path] -type f -group [group name]            | find /etc/server -type f -group teamstar                     |
| Find files modified after a specific date | find [directory path] -type f -newermt '[date and time]'     | find / -type f -newermt '6/30/2020 0:00:00'(all dates/times after 6/30/2020 0:00:00 will be considered a condition to look for) |
| Find files based on date modified         | find [directory path] -type f -newermt [start date range] ! -newermt [end date range] | find / -type f -newermt 2013-09-12 ! -newermt 2013-09-14(all dates before 2013-09-12 will be excluded; all dates after 2013-09-14  will be excluded, therefore this only leaves 2013-09-13 as the date to  look for.) |
| Find files based on date accessed         | find [directory path] -type f -newerat [start date range] ! -newerat [end date range] | find / -type f -newerat 2017-09-12 ! -newerat 2017-09-14(all dates before 2017-09-12 will be excluded; all dates after  2017-09-14 will be excluded, therefore this only leaves 2017-09-13 as  the date to look for.) |
| Find files with a specific keyword        | grep -iRl [directory path/keyword]                           | grep -iRl '/folderA/flag'                                    |
| read the manual for the find command      | man find                                                     | man find                                                     |



 **Note:** There are many more useful commands aside from the examples above. If  you ever have trouble understanding any of the syntax or getting it to  work, head on over to [explainshell.com](http://explainshell.com) to check the syntax and see how this tool can help you on your journey to Linux greatness. 

**Further notes:** if you do not know already, typing CTRL+L allows you to clear the screen  quicker rather than typing 'clear' all the time. Additionally, hitting  the up arrow allows you to return to a previously typed command so you  do not have to spend time retyping it again if you made an error. Cool.  Finally, placing: **2>/dev/null** at the end of your find command can help filter your results to exclude files/directories that you do not have permission to.

#  Working with files                            

You should be somewhat familiar already with working with files. Similar to windows, we can do the following:

- **copy files and folders**
- **move files and folders**
- **rename files and folders**
- **create files and folders**

For a quick recap to train your mental memory on the commands please refer to the below information:



| **What we can do**                                           | **Syntax**                                                   | **Real example of syntax**                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ | -------------------------------------------------- |
| copy file/folder                                             | cp [filename/folder] [directory](remember, if the filename/folder name has spaces then you will need to encase the filename with speech marks such as cp "[filename with spaces]"  [directory]. This applis to other commands such as mv. ) | cp ssh.conf /home/newfolder                        |
| move file/folder                                             | mv [filename] [directory]                                    | mv ssh.conf /home/newfolder                        |
| move multiple files/folders simultaneously                   | mv [file1] [file2] [file3] -t [directory to move to]         | mv logs.txt keys.conf script.py -t /home/savedWork |
| Move all files from current directory into another directory | mv * [directory to move files to]                            | mv * /home/scripts                                 |
| rename files/folder                                          | mv [current filename] [new filename]                         | mv ssh.conf NewSSH.conf                            |
| create a file                                                | touch [filename]                                             | touch newFile.txt                                  |
| create a folder                                              | mkdir [foldername]                                           | mkdir newFolder                                    |
| open file for editing                                        | nano [filename]                                              | nano keys.conf                                     |
| output contents of file                                      | cat [filename]                                               | cat keys.conf                                      |
| upload file to a remote machine                              | scp [filename] [username]@[IP of remote machine ]:/[directory to upload to] | scp example.txt john@192.168.100.123:/home/john/   |
| run an bash script program                                   | ./[name of script]                                           | ./timer                                            |
| open a file for reading/editing                              | nano [filename]                                              | nano readME.txt                                    |

A few additional things to remember is that occasionally you may  encounter files/folders with special characters such as `- (dash)`. Just  remember that if you try to copy or move these files you will encounter  errors because Linux interprets the - as a type of argument, therefore you will have to  place -- just before the filename. For example:` cp -- -filename.txt  /home/folderExample.`

# Hashing

Next we will talk about hashing, which is important for any Linux security researcher. Hashing refers to taking any data input, such as a password and calculating its hash equivalent. The hash equivalent is a long string which cannot be reversed since the act of hashing is known as a one-way function. For example, if you  visit: https://www.md5hashgenerator.com/ and type the following: mypassword123 or any other password, you will see how the hash algorithm known as MD5 will calculate and output a MD5 hash equivalent. Therefore, 'mypassword123' would have the MD5 hash equivalent of '**9c87baa223f464954940f859bcf2e233'.**

**
**

![img](https://i.imgur.com/syl6dNx.png)**
**![img]()**
**

**Why is hashing important?**





There are several reasons why hashing is important and I would encourage you to conduct some personal research after this, but the single most important use case you should concern yourself with in this room is integrity checking. For example, when you log into a website, your password must be checked with the local database to verify whether you should be allowed access. But think critically, if companies stored the username and password in plain text on the database, it would make it very easy for a successful hacker to be able to compromise every account. Therefore, it makes more sense to store the hash equivalent instead of storing the plain text credentials. So, when you send over your username and password to the database, the system will input these separately into a hash algorithm and if the output turns out to be equal to the hash equivalent stored in the database then it will allow you access. Sounds simple, right? If you are still confused, head over to this video and it should clarify any concerns: 



Note: MD5 and SHA1 are both examples of weak hash algorithms which have been proven to be vulnerable to something known as hash collision attacks which is explained further here: https://privacycanada.net/hash-functions/hash-collision-attack/. In short, do not use them because it has been proven that two different inputs can produce the same output (hash equivalent), thus, meaning that your password may produce the same hash as someone with a completely different password. Instead, SHA-256 is widely considered stronger as of today and is recommended. 

**How to crack hashes?
**

Hashes can be cracked through the method of brute-forcing. This essentially means  using a wordlist and inputting each potential password from the wordlist into  the hash function to see if we get a hash equivalent output that is equal to any of the hashes  stored in the database. However, in the example we store the hash in a text file  before specifying a wordlist to which we want to compare out calculated  hashes with. 

![img](https://i.imgur.com/KA6tcTZ.png)

Using a program called John the Ripper we can specify the format of the hash  we wish to crack (md5) the wordlist (rockyou.txt) and the wordlist  (hash.txt). Please see the full man page for garnering a more complete  understanding of all of the commands you can run with this program.

﻿![img](https://i.imgur.com/uNts45b.png)**
**



**Eventually** **John the Ripper may find the password if it was contained the wordlist. In the real world, you may have to find a larger wordlist with a strong amount of common password/username combinations.  In this example below the password was found to be 'password'.**

![img](https://i.imgur.com/Y7qZy3f.png)



Note: If you ever encounter a hash that you do not know the type of you can  use a tool called hash-identifier. However, with less widely used hashes the tool will not be accurate and therefore will still rely on you to  develop the skill of manually identifying what type of hash it is,  however this is out of the scope of this room. The syntax for  identifying unknown hashes is as so:

Hash-identifier [hash] as seen below in a real example:

![img](https://i.imgur.com/pmsYDlL.png)

Alternatively you could pipe the output of the file storing the hash to hash-identifier as shown below, which may be quicker.

![img](https://i.imgur.com/GiZAFRO.png)

The result should show us the most likely hash types that the hash most  likely is. As you can see there are two most probable hashes. In this  case the correct hash was in fact SHA-256, therefore you can see how in  most cases the first result is the correct answer, but please be aware  that this not always the case since many hash types can appear similar  in terms of string formatting.
![img](https://i.imgur.com/Mks9BiX.png)**
**
﻿﻿﻿﻿

**Note:** a more modern and powerful alternative to hash-identifer is a tool called haiti if you're interested: 
https://github.com/noraj/haiti

# Decoding base64                            

**What is base64**

Base64 is a group of binary-to-text encoding schemes that represent binary  data in an ASCII string format. In summary, it is just another way in  which data can be represented; some systems rely on a base64 encoding of data for processing while others may not. Head over to: https://www.base64encode.net/ and input any text and encode it. You should end up with something similiar below:

As you can see, the string 'example' gets coverted to the base64  'ZXhhbXBsZQ==' encoded string. This would allow certain systems to now  be able to read and process the data properly.

![img](https://i.imgur.com/RR3uS3d.png)

**Why should I care?**

There may be times when you encounter base64 converted data in files on a  system and needed to convert it to a human readable format. Therefore,  we can use the tool 'base64' with a pipe to translate it back to  something that is human readable. 

cat [filename] | base64 -d 

![img](https://i.imgur.com/JiGYvDY.png)

to transfer the output to a new text file simply use he > operator followed by the new filename.

![img](https://i.imgur.com/ahBMfSy.png)


Once again, encoding/decoding is changing data format. The data itself does not change, just how it reads. 

I encourage you to spend some time reading about encoding. At least  enough so you understand the difference between encoding/decoding vs  encryption/decryption as beginners sometimes confuse them with each  other.

#  Encryption/Decryption using gpg                            

**What is encryption/decryption**

Encryption refers to the process of concealing sensitive data by converting it to  an unintelligible format. The only way to reverse the process is to use a key; this is known as decryption. For further explanation please visit:https://www.cisco.com/c/en/us/products/security/encryption-explained.html but in short, encryption is just a way to protect data using a private  key. For example, the following string 'secret data' can be converted to 'QFnvZbCSffGzrauFXx9icxsN9UHHuU+sCL0sGcUCPGKyRquc9ldAfFIpVI+m8mc/'  using a key derived from the password 'pass'. It is also important to  note that there are many different types of encryption schemes, known as algorithms such as AES-256/128, 3DES, Blowfish, etc. Among these, AES  is considered to be the recommended encryption algorithm to use due to  the fact that it has been tested and proven to be a strong scheme.  Furthermore, there are two main types of encryption methods, asymmetric  and symmetric. However, in this room we will be focusing on symmetric  encryption. If you are interested in knowing the difference or more on  encryption please visit: https://www.thesslstore.com/blog/types-of-encryption-encryption-algorithms-how-to-choose-the-right-one/



**How to encrypt**

As seen below, we have a text file with sensitive information inside of it.**


![img](https://i.imgur.com/r29qByt.png)


This can be encrypted using the the program gpg to encrypt it using the AES-256 scheme:

**gpg --cipher-algo [encryption type] [encryption method] [file to encrypt]**

![img](https://i.imgur.com/L4WA3mz.png)


You will notice how it will ask for a password. This is when you can specify a password for gpg to derive the key from.


![img](https://i.imgur.com/mJyItsA.png)


Then a new encrypted file will be created with the extension gpg as you can see below.

![img](https://i.imgur.com/JvCMW5r.png) 


If you attempt to read the contents of this file you will see how it shows unintelligible code.

![img](https://i.imgur.com/cCaSV5m.png)

**How to decrypt**

Decrypting is very easy as it only takes one line as shown below:

**gpg [encrypted file]**

![img](https://i.imgur.com/Cd0uAIg.png)**


**Note:** You may notice how it does not ask us for the password to decrypt the file, this is because we we have already entered it when we were encrypting  the file and so Linux stored the password for quick use. However if we restart our system or  attempt to decrypt the file in a different linux machine, it will ask us for the password to decrypt the file.

**Remember:** You can always use the man pages to learn more about what you can do with gpg.

# Cracking encrypted gpg files                            

**How to crack encrypted files using john the ripper**

Now that you have a basic understanding using gpg, the next question is,  what if we do not have the password or key to decrypt the file? How can  we crack this. Well, similar to how we brute-forced the hashes in task 4 with John the Ripper, we can do the same for encrypted files. 

If you are using Kali linux or Parrot OS, you should have a binary add on called gpg2john. This binary program  allows us to convert the gpg file into a hash string that john the  ripper can understand when it comes to brute-forcing the password  against a wordlist. Simply type 'sudo gpg2john' into your terminal to  ensure you have it. The output should be as seen below. In any case if  you do not have it installed head over to: https://github.com/openwall/john and follow the instructions to install it. 

![img](https://i.imgur.com/7GsDmQ0.png)

Next, type the following command below to generate the hash for John the Ripper:

gpg2john [encrypted gpg file] > [filename of the hash you want to create]

![img](https://i.imgur.com/CZVKIlY.png)

The command above allows us to generate the hash for John the Ripper to  understand. Next we can begin the fun part of cracking the encrypted  file as seen below:

john wordlist=[location/name of wordlist] --format=gpg [name of hash we just created]

![img](https://i.imgur.com/wEvvEaY.png)

The result should reveal the password if you have used a strong wordlist that contains it. 

![img](https://i.imgur.com/YuJDBwV.png)

# Reading SQL databases                             

**What is SQL?**

SQL is a language for storing, manipulating and retrieving data from  databases. Therefore, it is important to firmly grasp the concept of how to read data from databases in Linux. If your understanding of databases is weak or you understanding nothing about them, please read this first before continuing: https://www.elated.com/mysql-for-absolute-beginners/ as it will explain fully the concept of databases for beginners. The key  thing to remember is that developers mostly create 'relational  databases' which use multiple databases that reference each other for  organising data, hence the name 'relational databases'. Furthermore,  each database contains tables of records and each table can consist of  multiple columns and rows which store the data in a organised format.  Now that we have gotten that out of the way let's begin.

Since this is a beginners room we will be reading the database of a local mysql workspace. This can be done as follows:

Service mysql start/stop

Start starts mysql while stop stops it. Additionally, you could use restart if you encounter any issues while mysql is running.

![img](https://i.imgur.com/GFEGVR2.png)



**Connect to remote SQL database:**

Mysql databases can be hosted for remote access. To access remote databases use the following command:

**mysql -u [username] -p -h [host ip]**

![img](https://i.imgur.com/hele1z6.png)

**Open SQL database file locally:**

To open mysql file/files locally, simply change to the directory of the  mysql file and type mysql as shown below. You'll be taken to a  specialised command prompt for mysql. 

Note: In some cases you may have to run mysql -p [password] if the mysql system was configured to require authenticiation.

1. **mysql -u [username] -p**

![img](https://i.imgur.com/5VRzbL5.png)

Type "source" followed by the filename of the mysql database to specify that you wish to view its database.

2. source [sql filename]

![img](https://i.imgur.com/qRuplIi.png)


**Displaying the databases**

After this, you will see how mysql takes a little time to load the database.  Once finished, type the following too see all of the relational  databases:

**SHOW DATABASES;**



![img](https://i.imgur.com/6yaf3oT.png)


**Choosing a database to view**

**We can select one of the databases by using the use command followed by  the name of it. In the example below we select the 'employees' database.**

**USE [database name]
**

![img](https://i.imgur.com/yaxykfA.png)



**Displaying the tables in the selected database**

We can display which tables inside that database we selected previously using:

**SHOW TABLES;**

![img](https://i.imgur.com/q2GyXX6.png)

Describing the table data structure

We can also view the table structure of individual tables using the DESCRIBE command:

**DESCRIBE [table name]**

![img](https://i.imgur.com/FdxFa6h.png)



**Displaying all the data stored in a specific table**

Now for the really fun part. Let's display all the data stored in the employees database using the following:

**SELECT * FROM [table name]**

![img](https://i.imgur.com/uQOo8Xs.png)

As seen below, we can see that this database contains some personal employee information.

![img](https://i.imgur.com/rPN7jv3.png)