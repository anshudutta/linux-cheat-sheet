## User management

### User
```
# list users
$ cat /etc/passwd
# format --> username:password:UID:GID:comments:home_dir:shell
# encrypted password is stored in /etc/shadow file (readable by root)

# root account has UID = 0, Sys accounts have UID < 1000
$ cat /ryc/login.defs

# the shell will be executed when user logs in
# check list of available shells
$ cat /etc/shells
# prevent interactive use of an account use /usr/sbin/nologin or /bin/false

# /etc/shadow
# format --> username: encrypted password: password info

# add user, -c comment, -m create home directory, -s shell path, -g primary group, -G other group
$ useradd -c "Joe Bill" -m -s /bin/bash -g sales joe

# assign password
$ passwd joe

# add service account
# -r sys account (uid <1000), -d home directory overide (for sys account), -u specify uid
$ useradd -c "Apache Web Server" -d /opt/apache -r -s /usr/sbin/nologin apache

# delete account
$ userdel joe -r
# modify using usermod
$ usermod -c "new comment"

# switch account
$ su - otheruser
```
### Group
```bash
$ cat /etc/group
# format --> group_name:password:GID:account1,accountN

# list what groups an user belongs to
$ groups root

# create group sales
$ groupadd sales
$ groupadd -g 2500 db

# delete group
$ groupdel db

# modify gpoupmod
$ groupmod -g 1001 -n newname
```
