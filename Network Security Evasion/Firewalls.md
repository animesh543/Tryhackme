#  Introduction                            

A firewall is software  or hardware that monitors the network traffic and compares it against a  set of rules before passing or blocking it. One simple analogy is a  guard or gatekeeper at the entrance of an event. This gatekeeper can  check the ID of individuals against a set of rules before letting them  enter (or leave).

Before we go into more details about firewalls, it is helpful to remember the contents of an IP packet and TCP segment. The following figure shows the fields we expect to find in an  IP header. If the figure below looks complicated, you don’t need to  worry as we are only interested in a few fields. Different types of  firewalls are capable of inspecting various packet fields; however, the  most basic firewall should be able to inspect at least the following  fields:

- Protocol
- Source Address
- Destination Address

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/09d8061f603e6ba8e65a185dc4a2d417.png)

Depending on the protocol field, the data in the IP datagram can be one of many options. Three common protocols are:

- TCP
- UDP
- ICMP

In the case of TCP or UDP, the firewall should at least be able to check the TCP and UDP headers for:

- Source Port Number
- Destination Port Number

The TCP header is shown in the figure below. We notice that there are many  fields that the firewall might or might not be able to analyze; however, even the most limited of firewalls should give the firewall  administrator control over allowed or blocked source and destination  port numbers.

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/756fac51ff45cbc4af49336b15a30928.png)



### Learning Objectives

This room covers:

1. The different types of firewalls, according to different classification criteria
2. Various techniques to evade firewalls

This room requires the user to have basic knowledge of:

- ISO/OSI layers and TCP/IP layers. We suggest going through the [Network Fundamentals](https://tryhackme.com/module/network-fundamentals) module if you want to refresh your knowledge.
- Network and port scanning. We suggest you complete the [Nmap](https://tryhackme.com/module/nmap) module to learn more about this topic.
- Reverse and bind shells. We recommend the [What the Shell?](https://tryhackme.com/room/introtoshells) room to learn more about shells.

The design logic of traditional  firewalls is that a port number would identify the service and the  protocol. For instance, visit [Service Name and Transport Protocol Port Number Registry](http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml) to answer the following questions.

#  Types of Firewalls                            

There are multiple ways to classify firewalls. One way to classify firewalls would be whether they are independent appliances.

1. Hardware Firewall (appliance firewall): As the name implies, an  appliance firewall is a separate piece of hardware that the network  traffic has to go through. Examples include Cisco ASA (Adaptive Security Appliance), WatchGuard Firebox, and Netgate pfSense Plus appliance.
2. Software firewall: This is a piece of software that comes bundled with the OS, or you can install it as an additional service. MS Windows has a  built-in firewall, Windows Defender Firewall, that runs along with the  other OS services and user applications. Another example is Linux  iptables and firewalld.

We can also classify firewalls into:

1. Personal firewall: A personal firewall is designed to protect a  single system or a small network, for example, a small number of devices and systems at a home network. Most likely, you are using a personal  firewall at home without paying much attention to it. For instance, many wireless access points designed for homes have a built-in firewall. One example is Bitdefender BOX. Another example is the firewall that comes  as part of many wireless access points and home routers from Linksys and Dlink.
2. Commercial firewall: A commercial firewall protects medium-to-large  networks. Consequently, you would expect higher reliability and  processing power, in addition to supporting a higher network bandwidth.  Most likely, you are going through such a firewall when accessing the  Internet from within your university or company.

From the red team perspective, the most crucial classification would  be based on the firewall inspection abilities. It is worth thinking  about the firewall abilities in terms of the ISO/OSI layers shown in the figure below. Before we classify firewalls based on their abilities, it is worthy of remembering that firewalls focus on layers 3 and 4 and, to a lesser extent, layer 2. Next-generation firewalls are also designed  to cover layers 5, 6, and 7. The more layers a firewall can inspect, the more sophisticated it gets and the more processing power it needs.

![img](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/84b1b779099b68c50c5685bc224a38db.png)

Based on firewall abilities, we can list the following firewall types:

- **Packet-Filtering Firewall**: Packet-filtering is the most basic  type of firewall. This type of firewall inspects the protocol, source  and destination IP addresses, and source and destination ports in the  case of TCP and UDP datagrams. It is a stateless inspection firewall.
- **Circuit-Level Gateway**: In addition to the features offered by  the packet-filtering firewalls, circuit-level gateways can provide  additional capabilities, such as checking TCP three-way-handshake against the firewall rules.
- **Next-Generation Firewal**: Compared to the previous types,  this type of firewall gives an additional layer of protection as it  keeps track of the established TCP sessions. As a result, it can detect and block any TCP packet outside an established TCP session.
- **Proxy Firewall** : A proxy firewall is also referred to as Application  Firewall (AF) and Web Application Firewall (WAF). It is designed to  masquerade as the original client and requests on its behalf. This  process allows the proxy firewall to inspect the contents of the packet  payload instead of being limited to the packet headers. Generally  speaking, this is used for web applications and does not work for all  protocols.
- **Next-Generation Firewall (NGFW)**: NGFW offers the highest firewall  protection. It can practically monitor all network layers, from OSI  Layer 2 to OSI Layer 7. It has application awareness and  control. Examples include the Juniper SRX series and Cisco Firepower.
- **Cloud Firewall or Firewall as a Service (FWaaS)**: FWaaS  replaces a hardware firewall in a cloud environment. Its features might  be comparable to NGFW, depending on the service provider; however, it  benefits from the scalability of cloud architecture. One example is  Cloudflare Magic Firewall, which is a network-level firewall. Another  example is Juniper vSRX; it has the same features as an NGFW but is  deployed in the cloud. It is also worth mentioning AWS WAF for web  application protection and AWS Shield for DDoS protection.

