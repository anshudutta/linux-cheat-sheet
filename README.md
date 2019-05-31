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
	echo somethime text to file (overwrites the file each time command is executed)
	```bash
	echo "some text" 1>somefile
	```
	OR
	```bash
	echo "some text" > somefile
	```
	append file
	```bash
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
```bash
# who is using?
w
# Show current processes
top
ps aux
# with pagination
ps aux | less
# show current network usage - tcp, udp, number
sudo netstat - tupln
```
