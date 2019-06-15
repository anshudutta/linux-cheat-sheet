# Linux Cheat Sheet
## Help
```
man <command>
```
## Shell
+ Linux process has 3 channels
	+ stdin (0)
	+ stdout (1)
	+ stderr (2)
+ Input Redirection
<b> prog1 < prog2 </b>

+  Output redirections
<b> prog1 > progr2 </b>
<b>prog1 >> progr2 </b>
	
	```bash
	# echo somethime text to file (overwrites the file each time command is executed)
	echo "some text" 1>somefile
	# OR
	echo "some text" > somefile
	# create or append existing file
	echo "some other text" >>somefile
	```
+ Pipes - Channel Output of one command to input another command
	<b>prog1 | prog 2 | prog 3</b>
	```bash
	ps aux | grep search | less
	```
+ && operator
	<b>prog1 && prog2</b>
	```bash
	ls -la && echo "success"
	```
+ grep
<b>prog1 | grep searchterm</b>
```bash
# search in the file
cat somefile | grep searchterm
# or
grep searchterm somefile
# search file with some content
grep searchterm ./*
# show unique filenames with search term
grep searchterm ./* | uniq | cut -d: f1
```
+ sort
```bash
ps aux | sort
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
* Navigation
```bash
cd /path/to/directory
# Go to root
cd /
# Go home
cd ~
# Print path
pwd
# . represents current directory
# Move one level up
cd ..
```
* Directory
```bash
ls
# List vertically
ls -l
# List Long, a = all, h = human readable
ls -lah
# List any directory
ls /abs/path/dir
# create a directort
mkdir somedir
# delete recurssively
rm -r somedir
```

* File
```bash
# create a file
touch somefile
# delete a file
rm somefire
# rename / move a file
mv /path/to/source/somefile /path/to/dest/
# copy a file
cp /path/to/source /path/to/dest
# to current directory
cp /path/to/source ./file
# read a file
cat somefile
# read / concat multiple files
cat somefile someotherfile
# read top of file
head -n 10 somefile
# read bottom of file
tail -f somefile
#  zip a file

# unzip a file
```

## File editor
### Vim --> vi improved (modal editor)
+ modes
	+ command mode --> mode for running commands
	+ insert mode ---> from command mode press i
+ commands (to be run in command mode)
	+ change to command mode --> press ESC
	+ quit --> <b>:q</b>
	+ write:--> <b>:w</b>
	+ write and exit --> <b>:wq</b>
	+ force quite --> <b>:q!</b> 
	+ set line number --> <b>:set number</b> 
	+ delete a line (n = number of lines) ---> <b>:ndd</b> 
	+ Undo last action ---> Press u
	+ Redo ---> <b>ctrl + r</b>
	+ Search ---> Type forward slash and then search term
		+ <b>/searchterm</b>
		+ Next --> <b>press n</b>
		+ Prev --> <b>press N</b>
	+ Search & Replace
		+ greedy ---> <b>:%s/searchterm/replaceterm/g</b>
		+ ask confirm ---> <b>:%s/searchterm/replaceterm/gc</b>
	
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
# add private key to known hosts
$ ssh-add /path/to/private/key
# add public key to authorozed hosts
$ echo $( cat /home/core/.ssh/id_rsa.pub) >> ~/.ssh/authorized_keys
# ssh specifying a key pair
$ ssh -i /path/to/private/key user@ipaddress 
# using username 
$ ssh user@ipaddress
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

### DNS
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
