# Nmap Live Host Discovery

### Introduction

Nmap scan steps:

1. Enumerate targets

2. Discover live hosts

3. Reverse-DNS lookup

4. Scan ports

5. Detect versions

6. Detect OS

7. Traceroute

8. Scripts

9. Write output

### Subnetworks

ARP packets cannot leave the subnet of the machine they are sent from. Packets generated by 
your scanner will be routed via the default gateway (router) to reach the systems on another 
subnet; however, the ARP queries won’t be routed and hence cannot cross the subnet router. 
ARP is a link-layer protocol, and ARP packets are bound to their subnet.

### Enumerating Targets

Use `nmap -sL <TARGETS>` to list the hosts nmap will scan or `nmap -iL <TARGETS-FILE>` to 
use a file as input.

### Discovering Live Hosts

Protocols for host discovery:

- ARP from Link Layer
- ICMP from Network Layer
- TCP from Transport Layer
- UDP from Transport Layer

If pinging a system on the same subnet, an ARP query should precede the ICMP Echo

### Nmap Host Discovery Using ARP

When no host discovery options are provided, Nmap uses the following approaches:

1. When a prvileged user tries to scan targets on a local network, nmap uses ARP requests.

2. When a privileged user tries to scan targets outside the local network, Nmap uses ICMP 
echo requests, TCP ACK to port 80, TCP SYN to port 443 and ICMP timestamp request.

3. When an unprivileged user tries to scan targets outside the local network, Nmap resorts to
a TCP 3-way handshake by sending SYN packets to ports 80 and 443

Run `nmap -PR -sn <TARGET-IP>/24` to perform an ARP scan without port scanning to 
discover all hosts on the subnet

The source address is the MAC address of our machine, while the destination is the 
broadcast address as we don’t know the MAC address of the target. However, we see the 
target’s IP address, which appears in the Info column. In the figure, we can see that we are 
requesting the MAC addresses of all the IP addresses on the subnet, starting with 
10.10.210.1. The host with the IP address we are asking about will send an ARP reply with 
its MAC address, and that’s how we will know that it is online.

An alternative is `arp-scan` [arp-scan Wiki](http://www.royhills.co.uk/wiki/index.php/Main_Page)

### Nmap Host Dicovery Using ICMP

Run `nmap -PE -sn <TARGET-IP>/24` to send ICMP echo requests to all IP addresses on the 
subnet. ICMP echo requests tend to be blocked, you can use ICMP Timestamp or ICMP Address 
Mask requests instead. 

Use `nmap -PP -n <TARGET-IP>/24` instead for ICMP Timestamp requests and use 
`namp -PM -sn <TARGET-IP>/24` for ICMP Address Mask requests.

### Nmap Host Discovery Using TCP and UDP

**TCP SYN Ping**

Run `nmap -PS -sn <TARGET-IP>/24` to use the TCP SYN ping. Privileged users do not need to 
complete the 3-way handshake even if the port is open; A TCP RST packet is sent instead of 
an ACK packet. Unprivileged users must complete the 3-way handshake. 

Use `-PS<PORT>` `-PS<PORT-RANGE>` `-PS<PORT>,<PORT>,<PORT>` to specify ports, default is 
port 80.

**TCP ACK Ping**

Run `nmap -PA -sn <TARGET-IP>/24` to use the TCP ACK ping. Must be run as a privileged user.

Use `-PA<PORT>` `-PA<PORT-RANGE>` `-PA<PORT>,<PORT>,<PORT>` to specify ports, 
default is port 80.

**UDP Ping**

Run `nmap -PU -sn <TARGET-IP>/24` to use UDP for host discovery. Sending a UDP packet to an 
closed port will get us an ICMP Port Unreachable packet as response telling us the host is 
up but that port is closed.

Use `-PU<PORT>` `-PU<PORT-RANGE>` `-PU<PORT>,<PORT>,<PORT>` to specify ports.

**Masscan**

Masscan uses a similar approach to discover the available systems. However, to finish its 
network scan quickly, Masscan is quite aggressive with the rate of packets it generates. 
The syntax is quite similar: -p can be followed by a port number, list, or range. Consider 
the following examples:

`masscan MACHINE_IP/24 -p443`
`masscan MACHINE_IP/24 -p80,443`
`masscan MACHINE_IP/24 -p22-25`
`masscan MACHINE_IP/24 ‐‐top-ports 100`

### Using Reverse-DNS Lookup

Nmap’s default behaviour is to use reverse-DNS online hosts. Because the hostnames can 
reveal a lot, this can be a helpful step. However, if you don’t want to send such DNS 
queries, you use -n to skip this step.

By default, Nmap will look up online hosts; however, you can use the option -R to query the 
DNS server even for offline hosts. If you want to use a specific DNS server, you can add 
the --dns-servers DNS_SERVER option.
