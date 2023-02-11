# Web Browser

**dev tools**

# Ping

Usage
  ping [options] <destination>

Options:
  <destination>      dns name or ip address
  -a                 use audible ping
  -A                 use adaptive ping
  -B                 sticky source address
  -c <count>         stop after <count> replies
  -D                 print timestamps
  -d                 use SO_DEBUG socket option
  -f                 flood ping
  -h                 print help and exit
  -I <interface>     either interface name or address
  -i <interval>      seconds between sending each packet
  -L                 suppress loopback of multicast packets
  -l <preload>       send <preload> number of packages while waiting replies
  -m <mark>          tag the packets going out
  -M <pmtud opt>     define mtu discovery, can be one of <do|dont|want>
  -n                 no dns name resolution
  -O                 report outstanding replies
  -p <pattern>       contents of padding byte
  -q                 quiet output
  -Q <tclass>        use quality of service <tclass> bits
  -s <size>          use <size> as number of data bytes to be sent
  -S <size>          use <size> as SO_SNDBUF socket option value
  -t <ttl>           define time to live
  -U                 print user-to-user latency
  -v                 verbose output
  -V                 print version and exit
  -w <deadline>      reply wait <deadline> in seconds
  -W <timeout>       time to wait for response

IPv4 options:
  -4                 use IPv4
  -b                 allow pinging broadcast
  -R                 record route
  -T <timestamp>     define timestamp, can be one of <tsonly|tsandaddr|tsprespec>

IPv6 options:
  -6                 use IPv6
  -F <flowlabel>     define flow label, default is random
  -N <nodeinfo opt>  use icmp6 node info query, try <help> as argument

# Traceroute

```bash
Traceroute <ip>
```

# Telnet

```
telnet 10.10.124.164 PORT   eg. 80, 23
```

# Netcat

| option | meaning                                                    |
|--------|------------------------------------------------------------|
| -l     | Listen mode                                                |
| -p     | Specify the Port number                                    |
| -n     | Numeric only; no resolution of hostnames via DNS           |
| -v     | Verbose output (optional, yet useful to discover any bugs) |
| -vv    | Very Verbose (optional)                                    |
| -k     | Keep listening after client disconnects                    |

# Putting It All Together

In this room, we have  covered many various tools. It is easy to put a few of them together via a shell script to build a primitive network and system scanner. You can use `traceroute` to map the path to the target, `ping` to check if the target system responds to ICMP Echo, and `telnet` to check which ports are open and reachable by attempting to connect to them. Available scanners do this at much more advanced and  sophisticated levels, as we will see in the next four rooms with `nmap`.

| Command          | Example                                      |
| ---------------- | -------------------------------------------- |
| ping             | `ping -c 10 10.10.185.142` on Linux or macOS |
| ping             | `ping -n 10 10.10.185.142` on MS Windows     |
| traceroute       | `traceroute 10.10.185.142` on Linux or macOS |
| tracert          | `tracert 10.10.185.142` on MS Windows        |
| telnet           | `telnet 10.10.185.142 PORT_NUMBER`           |
| netcat as client | `nc 10.10.185.142 PORT_NUMBER`               |
| netcat as server | `nc -lvnp PORT_NUMBER`                       |

Although these are fundamental tools, they are readily available on  most systems. In particular, a web browser is installed on practically  every computer and smartphone and can be an essential tool in your  arsenal for conducting reconnaissance without raising alarms. If you  want to gain more profound knowledge of the Developer Tools, we  recommend joining [Walking An Application](https://tryhackme.com/room/walkinganapplication).

| Operating System    | Developer Tools Shortcut |
| ------------------- | ------------------------ |
| Linux or MS Windows | `Ctrl+Shift+I`           |
| macOS               | `Option + Command + I`   |
