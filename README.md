# Linux Cheat Sheet
## Help
```
man <command>
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
```
* List a directory
```bash
ls
# List vertically
ls -l
# List all
ls -la
# List any directory
ls /abs/path/dir
```
* Create a directory
```bash
mkdir somedir
```
* Delete a directory
```bash
rm -r somedir
```
* Create a file
```bash
touch somefile
```
* Delete a file
```bash
rm somefire
```
* Move a file
```bash
mv /path/to/source/somefile /path/to/dest/
```
* Copy a file
```bash
cp /path/to/source /path/to/dest
# to current directory
cp /path/to/source ./file
```
* Read a file
```bash
cat somefile
```
* Read top of file
```bash
head -n 10 somefile
```
* Read bottom of file
```bash
# Follow the file for changes
tail -f somefile
```
* Zip a file
```bash
```
* Unzip a file
```bash
```
## File editor

## Sysadmin
* who is using?
```bash
w
```
* Show current processes
```bash
top
```
* Show current network usage
```bash
# tcp, udp, number
sudo netstat - tupln
```
