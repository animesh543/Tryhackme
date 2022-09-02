# Forensics - Analyse the PCAP

*For first two questions search for http stream or filter out all http request*.



1. What was the URL of the page they used to upload a reverse shell?                            

​      ![image-20220509124813490](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220509124813490.png)

2. What payload did the attacker use to gain access?

​       ![image-20220509124920327](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220509124920327.png)

3. What password did the attacker use to privesc?

​       For this task search for unusual (high number) ports in TCP protocol in this case it was

​      `tcp.dstport == 57680` 

 ![image-20220509132406001](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220509132406001.png)

4. How did the attacker establish persistence?

​       ![image-20220509132745965](C:\Users\pro\AppData\Roaming\Typora\typora-user-images\image-20220509132745965.png)

5. Using the fasttrack wordlist, how many of the system passwords were crackable?

   `4`

# Research - Analyse the code

For first two questions we need to vist the github link we found in task1 question4 

1. What's the default hash for the backdoor?                            

   var hash string = `bdd04d9bb7621687f5df9001f5098eb22bf19eac4c2c30b6f23efed4d24807277d0f8bfccb9e77659103d78c56e66d2d7d8391dfc885d0e9b68acd01fc2170e3`

2. What's the hardcoded salt for the backdoor?

​        return verifyPass(hash, `1c362db832f3f864c8c2fe05f2002a05`, password)

3. What was the hash that the attacker used? - go back to the PCAP for this!

`6d05358f090eea56a238af02e47d44ee5489d234810ef6240280857ec69712a3e5e370b8a41899d0196ade16c0d54327c5654019292cbfe0b5e98ad1fec71bed`

4. Crack the hash using rockyou and a cracking tool of your choice. What's the password?

   `november16`

used `hashcat -m 1710`

