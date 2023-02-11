| Protocol | TCP Port | Application(s) | Data Security |
| -------- | -------- | -------------- | ------------- |
| FTP      | 21       | File Transfer  | Cleartext     |
| HTTP     | 80       | Worldwide Web  | Cleartext     |
| IMAP     | 143      | Email (MDA)    | Cleartext     |
| POP3     | 110      | Email (MDA)    | Cleartext     |
| SMTP     | 25       | Email (MTA)    | Cleartext     |
| Telnet   | 23       | Remote Access  | Cleartext     |

**For man in the middle attack [Ettercap](https://www.ettercap-project.org) and [Bettercap](https://www.bettercap.org)**

```bash
scp <file/dir> name@machine_IP:/file location <save location>
eg
scp document.txt mark@MACHINE_IP:/home/mark
```

**hydra**

- `-l username`: `-l` should precede the `username`, i.e. the login name of the target.
- `-P wordlist.txt`: `-P` precedes the `wordlist.txt` file, which is a text file containing the list of passwords you want to try with the provided username.
- `server` is the hostname or IP address of the target server.
- `service` indicates the service which you are trying to launch the dictionary attack.

Consider the following concrete examples:

- `hydra -l mark -P /usr/share/wordlists/rockyou.txt MACHINE_IP ftp` will use `mark` as the username as it iterates over the provided passwords against the FTP server.
- `hydra -l mark -P /usr/share/wordlists/rockyou.txt ftp://10.10.168.53` is identical to the previous example. `MACHINE_IP ftp` is the same as `ftp://10.10.168.53`.
- `hydra -l frank -P /usr/share/wordlists/rockyou.txt 10.10.168.53 ssh` will use `frank` as the user name as it tries to login via SSH using the different passwords.

There are some extra optional arguments that you can add:

- `-s PORT` to specify a non-default port for the service in question.
- `-V` or `-vV`, for verbose, makes Hydra show  the username and password combinations that are being tried. This  verbosity is very convenient to see the progress, especially if you are  still not confident of your command-line syntax.
- `-t n` where n is the number of parallel connections to the target. `-t 16` will create 16 threads used to connect to the target.
- `-d`, for debugging, to get more detailed information  about what’s going on. The debugging output can save you much  frustration; for instance, if Hydra tries to connect to a closed port  and timing out, `-d` will reveal this right away.

| Protocol | TCP Port | Application(s)                  | Data Security |
| -------- | -------- | ------------------------------- | ------------- |
| FTP      | 21       | File Transfer                   | Cleartext     |
| FTPS     | 990      | File Transfer                   | Encrypted     |
| HTTP     | 80       | Worldwide Web                   | Cleartext     |
| HTTPS    | 443      | Worldwide Web                   | Encrypted     |
| IMAP     | 143      | Email (MDA)                     | Cleartext     |
| IMAPS    | 993      | Email (MDA)                     | Encrypted     |
| POP3     | 110      | Email (MDA)                     | Cleartext     |
| POP3S    | 995      | Email (MDA)                     | Encrypted     |
| SFTP     | 22       | File Transfer                   | Encrypted     |
| SSH      | 22       | Remote Access and File Transfer | Encrypted     |
| SMTP     | 25       | Email (MTA)                     | Cleartext     |
| SMTPS    | 465      | Email (MTA)                     | Encrypted     |
| Telnet   | 23       | Remote Access                   | Cleartext     |

Hydra remains a very efficient tool that you can launch from the  terminal to try the different passwords. We summarize its main options  in the following table.

| Option            | Explanation                                                  |
| ----------------- | ------------------------------------------------------------ |
| `-l username`     | Provide the login name                                       |
| `-P WordList.txt` | Specify the password list to use                             |
| `server service`  | Set the server address and service to attack                 |
| `-s PORT`         | Use in case of non-default service port number               |
| `-V` or `-vV`     | Show the username and password combinations being tried      |
| `-d`              | Display debugging output if the verbose output is not helping |