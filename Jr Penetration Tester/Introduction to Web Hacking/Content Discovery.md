# **Manual Discovery - Robots.txt**

**<ip>/robots.txt**

# Manual Discovery - Favicon

```shell-session
curl https://static-labs.tryhackme.cloud/sites/favicon/images/favicon.ico | md5sum    
```

# Manual Discovery - Sitemap.xml

 **<ip>/sitemap.xml **

# Manual Discovery - HTTP Headers

```shell-session
curl http://MACHINE_IP -v
```

# Manual Discovery - Framework Stack

look for **Framework Stack**

# OSINT - Google Hacking / Dorking

| **Filter** | **Example**        | **Description**                                              |
| ---------- | ------------------ | ------------------------------------------------------------ |
| site       | site:tryhackme.com | returns results only from the specified website address      |
| inurl      | inurl:admin        | returns results that have the specified word in the URL      |
| filetype   | filetype:pdf       | returns results which are a particular file extension        |
| intitle    | intitle:admin      | returns results that contain the specified word in the title |

# OSINT - Wappalyzer

install **Wappalyzer**

# OSINT - Wayback Machine

**website**   *https://archive.org/web/*

# OSINT - GitHub

*version control system*

# OSINT - S3 Buckets

*.s3.amazonaws.com*

One common automation method is by using the company name  followed by common terms such as **{name}**-assets, **{name}**-www, **{name}**-public, **{name}**-private, etc

# Automated Discovery

**Using ffuf:**

​            ffuf        

```shell-session
 ffuf -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt -u http://10.10.153.208/FUZZ
```

**Using dirb:**

​            dirb        

```shell-session
 dirb http://10.10.153.208/ /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
```

**Using Gobuster:**

​            gobuster        

```shell-session
 gobuster dir --url http://MACHINE_IP/ -w /usr/share/wordlists/SecLists/Discovery/Web-Content/common.txt
```
