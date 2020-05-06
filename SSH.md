## SSH
Generate key pair
```bash
$ ssh-keygen -C description@mail.com
```
Private key
```
# Add private key to known hosts using ssh agent
$ ssh-add /path/to/private/key
# add it manually
$ echo $(cat private.key) >> ~/.ssh/known_hosts
```
Public Key
```
$ ssh-copy-id -i ~/.ssh/mykey user@host
# OR
# add public key to authorized hosts on remote server
$ echo $( cat ~/.ssh/id_rsa.pub) >> ~/.ssh/authorized_keys
```
Login
```
# connect to remote host
$ ssh -i /path/to/private/key user@ipaddress
# using username
$ ssh user@ipaddress
```
Configuration
```
# ssh configuration (set up config in ~/.ssh/conf.d/)
$ echo "include ~/.ssh/conf.d/*.conf" >> ~/.ssh/config
```
### File copy
```bash
# scp --> secure file copy
$ scp filename user@servername:/path/to/destination

# sftp --> SSH file transfer protocol
$ sftp user@servername
$ lls 		# ls on (l)ocal, 
$ put filename 	# copies file to server
$ quit		quit
```
