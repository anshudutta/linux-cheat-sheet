# Linux Cheat Sheet

## Help

```
man <command>
man -k SEARCH_TERM
<command> --h or --help
```

## Shell

- Prompt
```bash
$ --> User
# --> Root
```
- Tilda - Represents home
```bash
~json 	# user home /home/json
~root 	# root home /root
~ftp 	# home of the srevice account /srv/ftp
```
- Environment variables
```bash
# PATH has list of path for all commands.
$ echo $PATH
$ echo $OLDPWD # the previous directory
$ export MY_VAR="some value"
$ unset MY_VAR
$ printenv
```
- Commands
```bash
# commands are case sensitive
$ which <command> # prints the path of that command
# Execute command (if command path is not in PATH)
$ /path/to/command # absolute path
$ ./command-in-current-dir # relative path
```
- Input/Output Types
```bash
I/O Name	Abbreviation	Value
-------------------------------------
Standard Input	stdin		0
Standard Output	stdout		1
Standard Error	stderr		2

Symbol	Description
---------------------
>	Redirects standard output to a file (Overwrites existing contents)
>>	Redirects standard output to a file (Appends existing contents)
<	Redirects input from a file to a command
&	Combines 

Examples
2>&1 		Redirects stderr to stdout
>/dev/null	Redirects to null device

# echo text and save stdout in a file
$ echo "some text" 1>somefile 
$ echo "some text" > somefile 
$ echo "some other text" >>somefile 	# Append
$ sort < files.txt 			# Input redirection
$ sort < files.txt > sorted_files 	# Combine Input and Output
$ ls not-here 2>/dev/null		# Send errors to null device
$ ls files.txt not-here > logs 2>&1	# Send output and error to logs file
```
- Pipes
```bash
# command-output | command-input
# takes stadout from left hand side and pipes it to stdin for command
$ cat file | grep pattern # same as grep pattern file
# search file with some content
$ grep searchterm ./*
# show unique filenames with search term
grep searchterm ./* | uniq | cut -d: f1
$ bash ps aux | grep search | less
```

- && Operator
```bash
$ bash ls -la && echo "success"
```
- Sort
```bash
ps aux | sort
```

### Bash Tricks

```bash
# check last exit code
$ echo $?
```

```bash
!#bin/bask

# stop on error
set -e

# call some function with all arguments
$ call-some-function $0
```

## Package Manager

### Ubuntu

```bash
# updates the list of available packages and their versions, w/o installing
sudo apt-get update
#  actually installs newer versions of the packages you have
sudo apt-get upgrade -y
# search for a package
apt-cache search tmux
# install
sudo apt-get install prog1 prog2 -y
# install w/o recommendation
sudo apt-get install prog1 prog2 --no-install-recommends
# uninstall
sudo apt-get remove prog
# browse package manager list source
cat /etc/apt/sources.list
# add a new repository to your package manager
sudo add-apt-repository ppa:libreoffice/ppa
```

## File System
Directory structure
```bash
/ -->					# Root
    /bin 				# Binaries and other exec programs
    /etc				# System config files
    /home				# Home directories
    /opt				# Optional or 3rd party sodtware
    /tmp				# Temporary space typically cleared on reboot
    /usr -->				# User related programs
	    /local -->			# Apps that are not shipped with distro
		     /app -->
		    	    /bin
			    /etc
			    /lib
			    /log
    /var				# Variable data, notably log files
    /dev				# Device files, typically controlled by system administrators
    /boot				# Files needed to boot operating system
    /lib				# System libraries
    /srv -->				# Contains data served by system, eg web server, services
	    /www			# Web server files
	    /ftp			# FTP files
```
- Navigation

```bash
cd /path/to/directory
# Go to root
cd /
# Go home
cd ~
# Print path
pwd
# .  represents current directory
# .. represents parent directory
# change to previous directory
cd - 
# Move one level up
cd ..
```

- Directory

```bash
$ ls
# List vertically
$ ls -l
# List Long, a = all, h = human readable
$ ls -lah
# List the time
$ ls -t
# Time in reverse
$ ls -r
# list the type
$ ls - F # ending / --> Directory, @ --> Link, * --> Executable
# List contents of directory recursively
$ ls -R
# List any directory
$ ls /abs/path/dir

# create a directort
$ mkdir somedir
# create a nested directory
$ mkdir -p somdir/anotherdir
# delete recursively with force
$ rm -rf somedir

# finding items
$ find .				# finds everything in the current directory
$ find /path/to/dir -name somename	# case sensitive search
$ find /path/to/dir -iname somename	# case insensitive search
$ find /path/to/dir -iname *somename	# wildcard search
$ find . -name s* ls			# find that starts with s and then perform ls
$ find . -type d -iname *some		# find type directory

# locate (uses an index)
$ locate somename
```

- File

```bash
# create a file
$ touch somefile
# delete a file
$ rm somefire
# rename / move a file
$ mv /path/to/source/somefile /path/to/dest/
# copy a file
$ cp /path/to/source /path/to/dest
# to current directory
$ cp /path/to/source ./file

# read a file with line number
$ cat -n somefile
# read / concat multiple files
$ cat somefile someotherfile
# read top of file
$ head -n 10 somefile
# read bottom of file
$ tail -f somefile

# Hidden files start with a dot (.)
$ touch .gitignore

# Symlink
$ 

# Difference between files
$ diff file1 file2 
# eg 3c3 --> line number file1 , (a)dd, (c)hange, (d)elete line number file2
# < --> first file, > --> second file
$ sdiff file1 file2 # diff side by side
$ vimdiff file1 file2 # ctrl-w w to move between windows

# Search in file --> Use grep
# i --> ignore case, c --> count number of instances, -n --> output with line number
$ grep searchterm file

#  zip a file

# unzip a file
```
- Wildcards (glob)

```bash
# asterix
# * --> *.txt, a*, a*.txt

# question Mark (matches one character)
# ? --> ?.txt, a?, a?.txt
```
- Permissions

```bash
$ ls -l 
# shows <type><user-permissions(rwx)><group-permissions(rwx)><other-permissions(rwx)>
# 1. Type
# d --> directory, - --> file, | --> symlink
# 2. Permissions -->
# u --> user(owner), g --> group(user in same group as owner), o --> other(not owner or in same group), a --> all
# r --> read, w --> write, x --> executable

$ groups # lists all groups
$ id -Gn
# show groups of user
$ groups <user>

# Setting permissions using chmod (change mode)
$ chmod <ugoa><+-><rwx> filename
# set write permission to all users in the same group as owner
$ chmod g+w ./sales.data
# remove write permission to all users in the same group as owner
$ chmod g-w ./sales.data
# Give users execute, remove execute from groups
$ chmod u+rwx,g-x ./sales.data

r	w	x
------------------
0	0	0 	# Binary Off
1	1	1	# Binary On
4	2	1	# Octal Mode

# Change the group of a file
$ chgrp <groupname> filename
```

- File editor

Vim --> vi improved (modal editor)

- modes 
command mode --> mode for running commands 
insert mode ---> from command mode press i

- commands (to be run in command mode) + 
change to command mode --> press ESC + quit --> <b>:q</b> + write:--> <b>:w</b> + write and exit --> <b>:wq</b> + force quite --> <b>:q!</b> + set line number --> <b>:set number</b> + delete a line (n = number of lines) ---> <b>:ndd</b> + Undo last action ---> Press u + Redo ---> <b>ctrl + r</b> + Search ---> Type forward slash and then search term + <b>/searchterm</b> + Next --> <b>press n</b> + Prev --> <b>press N</b> + Search & Replace + greedy ---> <b>:%s/searchterm/replaceterm/g</b> + ask confirm ---> <b>:%s/searchterm/replaceterm/gc</b>

## Services

Below is a simplified overview of the entire Linux boot and startup process:

1. The system powers up. 1 The BIOS does minimal hardware initialization and hands over control to the boot loader.
2. The boot loader calls the kernel.
3. The kernel loads an initial RAM disk that loads the system drives and then looks for the root file system.
4. Once the kernel is set up, it begins the systemd initialization system.
5. systemd takes over and continues to mount the host’s file systems and start services.

```bash
# enable a service 
$ systemctl enable `service-name`
# start a service
$ systemctl start `service-name`
# stop a service
$ systemctl start `service-name`
# check service status
$ systemctl status `service-name`
# Reload systemd manager configuration
$ systemctl daemon-reload
```

## Sysadmin

### Basic

```bash
# who is using?
w
whoami
# Show current processes
top
ps aux
# with pagination
ps aux | less
# show current network usage - tcp, udp, number
sudo netstat - tupln
```

### ssh

```bash
# generate ssh key pair
$ ssh-keygen -C description@mail.com
# add private key to known hosts using ssh agent
$ ssh-add /path/to/private/key
# add public key to authorozed hosts
$ echo $( cat /home/core/.ssh/id_rsa.pub) >> ~/.ssh/authorized_keys
# ssh specifying a key pair
$ ssh -i /path/to/private/key user@ipaddress
# using username
$ ssh user@ipaddress
```
### File copy
```bash
# scp --> secure file copy
$ scp filename user@servername:/path/to/destination

# sftp --> SSH file transfer protocol
$ sftp user@servername
$ lls 		# ls on (l)ocal, 
$ put file 	# copies file to server
$ quit		quit

```
### Certificates
Using Certificate Authority

- Creating root certificate
```bash
# Create private key for CA
openssl genrsa -out ca.key 2048

# Create CSR using the private key
openssl req -new -key ca.key -subj "/CN=KUBERNETES-CA" -out ca.csr

# Self sign the csr using its own private key
openssl x509 -req -in ca.csr -signkey ca.key -CAcreateserial  -out ca.crt -days 1000
```
- Creating client certificate
```bash
# Geenrate private key for admin user
openssl genrsa -out admin.key 2048

# Generate CSR for admin user. Note the OU.
openssl req -new -key admin.key -subj "/CN=admin/O=system:masters" -out admin.csr

# Sign certificate for admin user using CA servers private key
openssl x509 -req -in admin.csr -CA ca.crt -CAkey ca.key -CAcreateserial  -out admin.crt -days 1000
```
### System info

```bash
# disk usage, h --> human readable, s--> show size
$ sudo du -hs /path/to/directory
# check cpu
$ cat /proc/cpuinfo
# check cores
$ npproc
# check free space
$ free -h
```

### Networking

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

#### Link

```bash
# view network interfaces on host
$ ip link
# set up a network device
$ ip link set eth1 up
# down a network device
$ ip link set eth1 down
```

#### Ip address

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

#### Route tables

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
```
#### Ip Tables

```bash
# List all rules
$ sudo iptables -L
# BLock an ip address
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
#### DNS

```bash
# modify host file
$ echo "xxx.xxx.xxx.xx server-name" >> /etc/hosts
# view dns server settings
$ cat /etc/resolv.conf
# set up dns
$ echo "nameserver xxx.xxx.xxx.xxx" >> /etc/resolv.conf
$ echo "nameserver 8.8.8.8" >> /etc/resolv.conf
# debug dns
$ nslookup www.google.com
# nslookup using a specific name server
$ nslookup example.com ns1.nsexample.com
# nslookup using type
$ nslookup -type=soa example.com
```

#### namespaces

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

#### Setting up bridge network

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
  \*Interface for the host and switch to the namespaces

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
