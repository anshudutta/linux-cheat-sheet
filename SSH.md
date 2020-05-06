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
Port Forward
```
# ssh -L remote-port:localhost:local-port user@remoteip
$ ssh -L 1338:127.0.0.1:1338 anshu@192.168.231.145 -L 8000:127.0.0.1:8000 anshu@192.168.231.145
```
Reverse Port Forward
```
# Destination (192.168.20.55) <- |NAT| <- Source (138.47.99.99)
$ ssh -R 19999:localhost:22 sourceuser@138.47.99.99 # run on the destination
$ ssh localhost -p 19999 # run on local
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
