# OSINT - SSL/TLS Certificates

*http://crt.sh/*

*https://transparencyreport.google.com/https/certificates*

# OSINT - Search Engines    

example **-site:www.tryhackme.com site:\*.tryhackme.com**

# DNS Bruteforce

dnsrecon -t brt -d <site/ip>

# OSINT - Sublist3r

*./sublist3r.py -d <site/ip>*

# Virtual Hosts

```shell-session
ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://MACHINE_IP
        
```

The above command uses the **-w** switch to specify the wordlist we are going to use. The **-H** switch adds/edits a header (in this instance, the Host header), we have the **FUZZ** keyword in the space where a subdomain would normally go, and this is where we will try all the options from the wordlist. 

Because the above command will always produce a valid result, we need to filter the output. We can do this by using the page size result with the **-fs** switch. Edit the below command replacing {size} with the most occurring size value from the previous result and try it on the AttackBox.        

```shell-session
 ffuf -w /usr/share/wordlists/SecLists/Discovery/DNS/namelist.txt -H "Host: FUZZ.acmeitsupport.thm" -u http://MACHINE_IP -fs {size}
        
```

This command has a similar syntax to the first apart from the **-fs** switch, which tells ffuf to ignore any results that are of the specified size.