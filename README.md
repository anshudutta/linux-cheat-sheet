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
# . represents current directory
# Move one level up
cd ..
```
* List a directory
```bash
ls
# List vertically
ls -l
# List Long, a = all, h = human readable
ls -lah
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
* Read a multiple files / concatenate
```bash
cat somefile someotherfile
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
