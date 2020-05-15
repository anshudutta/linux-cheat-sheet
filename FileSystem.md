## File System
#### Directory structure
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
### Navigation

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

### Directory

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

### File

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
### Wildcards (glob)

```bash
# asterix
# * --> *.txt, a*, a*.txt

# question Mark (matches one character)
# ? --> ?.txt, a?, a?.txt
```
### Permissions

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

### File editor

Vim --> vi improved (modal editor)

```bash
- modes 
command mode --> mode for running commands 
insert mode ---> from command mode press i

- commands (to be run in command mode)
change to command mode --> press ESC 
- quit --> <b>:q</b> 
- write:--> <b>:w</b> 
- write and exit --> <b>:wq</b> 
- force quite --> <b>:q!</b> 
- set line number --> <b>:set number</b> 
- delete a line (n = number of lines) ---> <b>:ndd</b> 
- Undo last action ---> Press u 
- Redo ---> <b>ctrl + r</b> 
- Search ---> Type forward slash and then search term <b>/searchterm</b> 
	- Next --> <b>press n</b> 
	- Prev --> <b>press N</b> +
- Search & Replace + greedy ---> <b>:%s/searchterm/replaceterm/g</b> 
- ask confirm ---> <b>:%s/searchterm/replaceterm/gc</b>
```
### Disk Management

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

```
Disk Information
```
# show the file system usage (disk free)
$ df -h

# show info on all block devices
$ lsblk

# find all mount paths
$ findmnt

# find disk usage on a path
$ du /path/to -h
```
