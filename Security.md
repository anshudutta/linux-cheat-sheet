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

## PAM
Pluggable Authentication module
```bash
login ---> PAM --> /etc/passwd
                   /etc/shadow
```
Location
```bash
$ ls -lah /etc/pam.d
$ cat /etc/pam.d/login
$ cat /etc/pam.d/sshd
```
Forrmat
```bash
module_interface control_flag module_name module_args

module_interface --> auth (authentication), account(authorization), password (change password), session (manage session)
control_flag --> required, requisite, sufficient, optional, include
```
Example
```
#% PAM-1.0
auth required pam_securetty.so
auth required pam_unix.so nullok
auth required pam_nologin.so
account required pam_unix.so
password required pam_unix.so retry=3
session required pam_unix.so
```
## Best practices
- Disable root login
- Use sudo to run commands that require root priviledges. Avoid using `su - root` or `sudo` to gain root shell. This is for     better audit trail on who ran what command
- Disable ssh root logins
  ```bash
  $ vim /etc/ssh/sshd_config
  # PermitRootLogin no
  $ sudo systemctl reload sshd
  ```
- Use one service account per service
- Deny login, ssh from service accounts
- Delete unused service account

