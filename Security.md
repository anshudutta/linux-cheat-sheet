## Security

- Multiuser system
- Superuser is the root account
- Other accounts are non root accounts
- File permissions, every file has an owner, can't be modified by other users w/o explicit grant
- Process permissions, every process has an owner
- Centralized software management/package distribution

### Physical security
#### Secure Single User mode 
Single user mode is a mode in which a multiuser computer operating system boots into a single superuser.
Make sure password is required for single user mode

Ubuntu - https://askubuntu.com/questions/1011368/how-can-i-protect-against-single-user-mode

#### Secure Boot Loader
Boot Loader --> Loads Kernels ---> Loads init.d
We can ask the boot loader to replace init.d with bash bypassing security
Set a password on boot loader to make any changes
```
$ cd /etc/grub.d
$ cat <<EOF >> /etc/grub.d/40_custom
set superusers="root"
password root root
EOF
# encrypt the file
# rebuild grub.cfg file
```
#### Disk Encryption
Using dm-crypt
```
--------------------------------------
File system and Directories /home
--------------------------------------
--------------------------------------
File system EXT4
--------------------------------------
--------------------------------------
Virtual Block Device /dev/mapper/home
--------------------------------------
--------------------------------------
Encryption/Decryption dm-crypt
--------------------------------------
--------------------------------------
Physical Block Device /dev/sda2
--------------------------------------
```
Using LUKS
- Linux Unified Key Setup
- Front-end for dm-crypt
- Multiple passphrase support
- Portable as LUKS stores setup information in partition header
- Great for removable device
