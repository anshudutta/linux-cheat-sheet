### IP Tables
```bash
PACKET IN
    |
PREROUTING--[routing]-->--FORWARD-->--POSTROUTING-->--OUT
 - nat (dst)   |           - filter      - nat (src)
               |                            |
               |                            |
              INPUT                       OUTPUT
              - filter                    - nat (dst)
               |                          - filter
               |                            |
               `----->-----[app]----->------'
```

```bash
# List all rules
$ sudo iptables -L
# Block an ip address
$ sudo iptables -A INPUT -s `soure ip` -j DROP
# Reject a connection --> connection refused
$ sudo iptables -A INPUT -s `soure ip` -j REJECT
# Block connections to a network interface
$ iptables -A INPUT -i eth0 -s `source ip` -j DROP
# Allow ssh from a specific subnet and outgoing traffic of established SSH connections
$ sudo iptables -A INPUT -p tcp -s 15.15.15.0/24 --dport 22 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
# outgoing traffic of established SSH connections only if the OUTPUT policy is not ACCEPT
$ sudo iptables -A OUTPUT -p tcp --sport 22 -m conntrack --ctstate ESTABLISHED -j ACCEPT
# Allow all incoming HTTP and HTTPS
$ sudo iptables -A INPUT -p tcp -m multiport --dports 80,443 -m conntrack --ctstate NEW,ESTABLISHED -j ACCEPT
$ sudo iptables -A OUTPUT -p tcp -m multiport --dports 80,443 -m conntrack --ctstate ESTABLISHED -j ACCEPT
```


### NAT
- SNAT
- DNAT
- PAT
- PORT FORWARD

1. To enable NAT you need to have 2 network interfaces - private and public
2. Using PAT assign a public ip address to a packet coming from a private network for outgoing request
3. For response, allow the incoming traffic to be forwarded to the correct host in private network

```bash
# enable forwarding in a router
$ echo 1 > /proc/sys/net/ipv4/ip_forward
# allow on start up
$ echo "net.ipv4.ip_forward=1" >> /etc/sysctl.conf
$ sysctl -p

# setup NAT, eth0 --> external n/w, eth1 --> internal n/w
$ iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
$ iptables -A FORWARD -i eth0 -o eth1 -m state --state RELATED,ESTABLISHED -j ACCEPT
$ iptables -A FORWARD -i eth1 -o eth0 -j ACCEPT
$ iptables save
```

```bash
# iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 80 -j DNAT --to 192.168.1.2:8080
# iptables -A FORWARD -p tcp -d 192.168.1.2 --dport 8080 -j ACCEPT
```
