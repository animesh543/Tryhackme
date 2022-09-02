# Introduction                            

A computer network is a group of computers and devices connected with each other. Network  security focuses on protecting the security of these devices and the  links connecting them. (In more precise terms, network security refers  to the devices, technologies, and processes to protect the  confidentiality, integrity, and availability of a computer network and  the data on it.)

Network security consists of different hardware and software  solutions to achieve the set security goals. Hardware solutions refer to the devices you set up in your network to protect your network  security. They are hardware, so you can literally hold them. A hardware  appliance might look something like the image below.

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5a7527aeef27ad74320defaf1bae8d6d.png)

Examples of hardware appliances include:

- Firewall appliance: The firewall allows and blocks connections based on a predefined set of rules. It restricts what can enter and what can  leave a network.
- Intrusion Detection System (IDS) appliance: An IDS detects system  and network intrusions and intrusion attempts. It tries to detect  attackers’ attempts to break into your network.
- Intrusion Prevention System (IPS) appliance: An IPS blocks detected  intrusions and intrusion attempts. It aims to prevent attackers from  breaking into your network.
- Virtual Private Network (VPN) concentrator appliance: A VPN ensures  that the network traffic cannot be read nor altered by a third party. It protects the confidentiality (secrecy) and integrity of the sent data.

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/ab8ec75d3343f5d49c616f6b31840e49.png)

On the other hand, we have software security solutions. Common examples are:

- Antivirus software: You install an antivirus on your computer or  smartphone to detect malicious files and block them from executing.
- Host firewall: Unlike the firewall appliance, a hardware device, a  host firewall is a program that ships as part of your system, or it is a program that you install on your system. For instance, MS Windows  includes Windows Defender Firewall, and Apple macOS includes an  application firewall; both are host firewalls.

![Windows Defender Firewall](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/e2ecef770b63d3af285688bb5e141bbc.png)

According to the [Cost of a Data Breach Report 2021](https://newsroom.ibm.com/2021-07-28-IBM-Report-Cost-of-a-Data-Breach-Hits-Record-High-During-Pandemic) by IBM Security, a data breach in 2021 cost a company $4.24 million per incident on average, in comparison with $3.86 million in 2020. The  average cost changes with the sector and the country. For example, the  average total cost for a data breach was $9.23 million for the  healthcare sector, while $3.79 million for the education sector.

# Methodology                            

Every “operation” requires some form of planning to achieve success.  If you are interested in wildlife photography, you cannot just grab a  camera and head to the jungle unless you don’t care about the outcome.  For a safe and successful wildlife photography tour, you would need to  learn more about the animals you want to shoot with your camera. This  includes the habits of the animals and the dangers to avoid. The same  would apply to a military operation against a target or breaking into a  target network.

![Cyber Kill Chain](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/9cba92802ccc445a7690301b3ea49283.png)

Breaking into a target network usually includes a number of steps. According to [Lockheed Martin](https://www.lockheedmartin.com/en-us/capabilities/cyber/cyber-kill-chain.html), the Cyber Kill Chain has seven steps:

1. Recon: Recon, short for reconnaissance, refers to the step where the attacker tries to learn as much as possible about the target.  Information such as the types of servers, operating system, IP  addresses, names of users, and email addresses, can help the attack’s  success.
2. Weaponization: This step refers to preparing a file with a malicious component, for example, to provide the attacker with remote access.
3. Delivery: Delivery means delivering the “weaponized” file to the  target via any feasible method, such as email or USB flash memory.
4. Exploitation: When the user opens the malicious file, their system executes the malicious component.
5. Installation: The previous step should install the malware on the target system.
6. Command & Control (C2): The successful installation of the  malware provides the attacker with a command and control ability over  the target system.
7. Actions on Objectives: After gaining control over one target system, the attacker has achieved their objectives. One example objective is  Data Exfiltration (stealing target’s data).

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/a8e61f1f9a2ea3612d3bf84f9a11f41c.png)

Another analogy would be a thief interested in a target house. The  thief will spend some time learning about the target house, who lives  there, when they leave, and when they return home. The thief will  determine whether they have security cameras and alarm systems. Once  enough information has been gathered, the thief will plan the best  entrance strategy. Physical theft planning and execution resemble, in a  way, the malicious attack that aims to break into a network and steal  data.

# Practical Example of Network Security

