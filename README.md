# Linux Cheat Sheet

## Help

```
man <command>
man -k SEARCH_TERM
<command> --h or --help
```

## Shell

### Prompt
```bash
$ --> User
# --> Root
```
### Tilda - Represents home
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
### Commands
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
### Pipes
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
### Processes
```bash
# display process with current session
$ ps
# a = all users, u = display user/owner, x = processes not attached to a terminal
$ ps aux
# display all processes, full
$ ps -ef
# display a process tree
$ ps -e --forest
# display users processes
$ ps -u --username
# display process in tree format
$ pstree
# Interactive process viewer
$ top
$ htop

# kill
$ kill <pid>
$ kill -9 <pid> # force kill
$ pkill processname
```

### && Operator
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
# hostname
$ hostname
```
### Logs
- Provides centralized logging so that applications don't have to manage logs
- Applications can use `logger` tool to write logs

Log Servers
- syslogd
- rsyslog
- syslog-ng

Configuration
```bash
$ echo "include /etc/syslog.d/*.conf" >> /etc/syslog.conf
```

Log 
```bash
# check all logs
$ less /var/logs/syslog

# boot --> /var/logs/dmesg
# boot --> /var/logs/boot.log
# cron --> /var/logs/cron
```
### Disk Usage

- To install an operating system on a hard drive, it must be subdidvided into dstict storage unit called partitions
- Hard drives in linux are named as /dev/sda, /dev/sdb ..etc
- Partitions in linux are named using numbers strating from 0. eg- sda1, sda2,..sda4 
- A mount point is a directory used to access data on partition. Most commonly used mount points are /, /boot, /home/ etc
- Partitioning schemes
   - MBR
   - GPT

Hard Drive --> Partition --> Install File System --> Mount --> block Device

```
Partition	Type	Mount Point
------------------------------------
/dev/sda1	ext2	/
/dev/sda2	ext2	/boot
/dev/sda3	ext4	/user
/dev/sda4	ext4	/home
```

```bash
# commands --> fdisk, parted, gdisk
# display a list of disks
$ fdisk -l

# creating a partition --> fdisk /path/to/disk and follow commands
# choose p --> primary, 1 --> number, +1G for 1 GB in size, m--> help, w --> write
$ fdisk /dev/sdb

# creating a file system 
# mkfs -t TYPE DEVICE
$ mkfs -t ext4 /dev/sda1

# mount the partition
# mount DEVICE MOUNT_POINT
$ mount /dev/sdb3 /opt
# persist the change in /etc/fstab
# unmount
$ umount /opt

# show the file system usage (disk free)
$ df -h

# show info on all block devices
$ lsblk
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
