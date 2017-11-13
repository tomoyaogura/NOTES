# Personal Setup
Emmet/ Typescript installation on sublime
Augry


# Unix
## Chapter 1
### Commands
```
which
whereis
logout
exit
tty (teletype terminal)
ps -a all -u user_format -x all_processes -f tree_format

```
### Concepts:
- Kernel interacts with Hardware and shell [OS]
- Shell interacts between Kernel (with syscalls) and User
- POSIX [Portable OS Interface for Computer Environments): Specifies UNIX syscalls to implement
- UNIX: Ken Thompson, Dennis Ritchie
- LINUX: Linux Torvalds
- Shell: bourne (sh) bourne again (bash) korn (ksh) POSIX (sh)

## Chapter 2
### Commands
```
man: 1-user_prog 2-kernel_syscall 3-lib_fxn 4-special_file 5-admin_file_format 6-games 7-macro_pkg 8-admin
echo
whoami
passwd
uname --all --nodename -r linux_version
who 
date
```
### Signals:
|Shortcut | Action 		|
|---------|--------		|
|[Ctrl-\\] | Quit		|
|[Ctrl-z] | Suspend		|	
|[Ctrl-c] | Interrupt 	|
|[Ctrl-d] | Terminate stdin (eof)|

## Chapter 3: Filesystem
### Commands
```
ls [-d */]
pwd  
cd
rmdir
mkdir
ls: -i inode -x column -R recursive -a all
cp
mv
rm -r recursive -f force
wc -l lines 
tar [-c create |-x extract] -t displays_file -v progress -f filename -z compress
tar -cvfz/ -xvfz
```

### Concepts
* Directory, Ordinary, Device
  * Directory: Filename + inode
  * Ordinary: Txt/ Executable
  * Device: Stream
* Filename: Alphanumeric, 255, case sensitive
* Directory:
  * /bin + /usr/bin: Binaries
  * /sbin /usr/sbin: System executables
  * /etc: Config
  * /dev: Devices
  * /lib + /usr/lib: Library files
  * /usr/include
  * /tmp: Temporary
  * /var: Variable - mail
  * /home: User files

## Chapter 4: File Attributes
### Commands
```
chmod
chgrp
chown
umask: Default mask [666/777 - umask(022)]
ln [file] [hard_link] 
ln -s soft_link [file/dir] [file/dir_2]
find path criteria [Default -name] | -type -size- user- etc.] action [-print] 
ls -lt [Order modification]
```
#### Concepts
* Permissions: user-group-others

| |Read|Write|Execute|
|----|----|-----|-------|
|Directory|List dir_entry (ls)| Create remove file (rm, cp, mv)| Searching (cd)/ Openin|
|File|Read|Save, write| Execute|
* To write a file you need to traverse - have execute permissions
* To read a file you can still open file without exec permissions, but not dir_entry

* Inode has: File type, permissions, links, UID, GID, size, modified, last acccess, etc. 
* Hard Links: Creates copy reference to inode -> Need to delete both refs to delete ++link_ct
* Soft link/ symlinks: Different inode with path
* Find Advanced:

## Chapter 7: Shell
### Concepts
* Process
  * Prompt
  * Parse metacharacter
  * Execute
  * Wait
  * Prompt
* Wild cards:
  * \*:	 matches all non dot or slash starting
  * ?:	matches single char
  * [a-z]: matches char class a-z
  * {list, list2}: matches list
  * \\: Escapes
  * ': Escapes all
  * ": Escapes but `` and $
  * ``: Code execution
* Redirection:
  * 0 - stdin, 1 - stdout, 2 - stderr
  * \>: Create/overwrite
  * \>\>: Append
  * 2>: pipes stderr to x
  * <: Input redirect 
  * 1>&2: Redirects stdin to stderr
  * /dev/null: accepts void
* Grouping:
  * ()/{} - sends all to command group (same redirecting)
* Arguments
  * thing1 > thing2 - Takes file, passes as stdin stream
  * -Run thing1
  * -Out thing1 to file thing2
  * | - Passes stdout as stdin -> thing1 > temp && thing2 < temp 
  * Passing filename/ text to argument: Use \`\` or $() 
  * tee - Filter: prints out to file and prints on terminal
  * xargs - Obtains file list from stdin, executes -n batches at once
  * Ex: `find . -name "*.txt" | xargs rm`

## Chapter 8: Processes
### Commands
```
ps -f PPID -u usr -a
kill
& - run in background
$! - PID of last background job
nohup - run after logout
fg %1
bg
suspend [Ctrl-z]
jobs
$$ - Shell PID
```

### Concepts
* init process spawns everything [Like shell]
* daemons: Sleep until active
* wait, exec, fork
* processes inheret: fd, uid, eff_uid, env [exported], cwd
* () runs in subshell, {} runs in current shell
* Process Status:
  * [O] running: Using CPU
  * [R] runnable: Ready queue
  * [S] sleeping: Waiting for IO
  * [T] suspended: Ctrl-z
  * [Z] zombie: parent didn’t wait for their death
  * orphan:
* Signals
  * [1] SIGHUP
  * [2] SIGINT: interrupt
  * [3] SIGQUIT: quit 
  * [9] SIGKILL: force kill
  * [15] SIGTERM: kill
  * [20] SIGSTP: Suspend
* PID & PPID (Parent)
* crontabs: `Min Hour Day Month Week command`

## Chapter 9: Shell
### Commands
```
chsh (CHange SHell)
env (inheret process)
set (built-in, )
export VAR=$VAR
alias alias="command"
watch -n 1 “COMMAND”
```

### Concepts

* .profile - runs at login
* .rc - runs at shell

## Chapter 10: Simple Filters
### Comparing Files
* `cmp group1 group2 [-l all matches]`
* `comm group[12].sorted`
  * sort group1 > group1.sorted; sort group2 > group2.sorted
* comm -column_to_exclude
  * Column 1: first file, Column 2: second file, Column 3: third file
* diff group1 group2
  * change/ append line x to line x

### Reading Files
* head [-n how many lines]
`` vim `ls -t | head -n 1` `` -> Vim opens most recent file
* Use with grep
* tail: Bottom [-n num_lines] [-f growth]
* cut [-d delimiter] [-f fields]
* paste [-s newlines] [-d add delimiter]

### More Files
* sort
  * -t delimiter + -k key_num
  * -u remove repeats
  * -c check if sorted
  * -r reverse sort
  * -n numerical sort
  * `ls -l | sort -k 5 -nr | head -n 5 | tr -s " " | cut -d" " -f5,9
* uniq
  * -u picks unique lines
  * -d sellects dup lines
  * -c count frequency
* tr
  * `tr options exp1 exp2 std_in`
  * Ex:`head -n 3 file | tr '[a-z]' '[A-Z]'
  * -d delete
  * -s compress repeating
  * -cd delete all except

## Chapter 11: Grep & Sed
### Concepts
* BasicRegularExpression vs ExtendedRegularExpression:
* ^ is not class if in char class
* ^beginningend$
* **\* 0 or + of PREV CHAR, not WILD CARD**
* ls | grep *.py VS ls | grep "*.py"
* ERE (-E):
* ? zero or one, + one or more
* () and | 

### GREP

* -i: Case ignore
* -n: Line number
* -l: File name only
* -v :Inverse selection
* `ps -auxf | pgrep python`

### sed option 'address action' files
* Address selection: Address: two line numbers x,y OR /pattern/,/pattern/
* Only BRE regex
* -d: Delete lines
* -i,a,c: insert/append/change
* s/s1/s2/g
* `sed '/This/,/That/w forms.html' file`: Writes excerpt to forms.html
* `sed -n '/From: /p file` - Print all lines From:
* `sed '/^\/\/.*;/d' test1` - Remove all commended out c code
* `sed '1i #include<studio.h>' file > file2` - Appends file
* `sed '/grep/s//member/' file` - Uses remembered pattern
* `sed -i.bk -f sed.script file` - Does infile replacement with backup file

#### Repeated Patterns
* `sed 's/director/executive &/' file`: Replaces director with exec. director
* Interval RegEx: 
  * ch\{m}: Character m times
  * ch\{m,}: Charater at least m times
  * ch\{m,n}: Character between m n times
* `grep '^.\{120,\}'`: Finds any lines longer 120 
* Tagged RegEx: Wrapped in \(\) can be referenced by \1 \2...

## Chapter 14: Remote
### Commands:
```
hostname
ftp/sftp [-r]
scp
ssh login [command (escape wildcards to eval remotely)]
telnet
smtp
http
pop3
ping
```

### Concepts:
* hostnames resovled at /etc/hosts
* DNS deals with ip resolution in hiearchial format

### FTP:
```
pwd, ls, cd, mkdir, rmdir, chmod & lpwd, lls, lmkdir
put local.gif remote.gif
mput *multi.files -> directory or cwd
get remote.gif local.gif
mget *multi.files -> directory or cwd
```

### SSH:
```
ssh-agent $(ssh-eval -s)
```

## Shell Programming:
### Syntax:
* For loop:
```
for variable in list
do
  commands
done
```

* If statements:
```
if [condition ] && [condition]
then
  commands
elif 
then
  command
else
then
  command
fi
```

EX: Argument Checking
```
if [ -n "$1" ]; then
  echo "Has one arg"
fi

if [[ ${#} != 4 ]]; then

```

* Case:
```
case `basename $0` in    # Or:    case ${0##*/} in
    "wh"       ) whois $1@whois.tucows.com;;
    "wh-ripe"  ) whois $1@whois.ripe.net;;
    "wh-apnic" ) whois $1@whois.apnic.net;;
    "wh-cw"    ) whois $1@whois.cw.net;;
    *          ) echo "Usage: `basename $0` [domain-name]";;
esac 
```
* Operators:
* -ne not equal
* -n 
* !=: Strings
* -z $Unassigned (String length zero)
* -n $String (String length nonzero)
* -d: $File (File exists and directory)
* -e: $File (File exists)
* \`\` -> $()
* (( )): Arithmetic evaluation *EXIT STATUS OPPOSITE (( 0 )) -> 1 (( 1 )) -> 0
* [ NULL && $EMPTY && $false ]: Only ones evaluate false; 0 evals TRUE

### Tricks:
* || to run second ONLY if succeeds
* $# Num of arguments passed
* $@: List arguments in string [shift drops first]
* $-: List of flags
* $? Exit status
* $$ PID
* ~+ cwd
* ~- prev dir
* ! exit status -> Determine 

### Special Chars
* ; Allows two commands in one line
* : No-op
* ? condition?result-if-true:result-if-false
* $[number ops]: Numerical ops

### Concepts
* Untyped Var

### String Manipulation
* ${str:0:2} : str[0:2]
* ${str:2} : str[2:END]
* ${#str}: len(str)
* ${str#pat1} : Remove shortest front pat1
* ${str##pat1} : Remove long matches front pat1
*  expr index "$str" str
* ${str//pat1/pat2}: Replace ALL pat1 with pat2
* ${str/pat1/pat2}: Replace first pat1 with pat2
* ${var-default}

# Docker
## Docker system:
`Docker server/ engine/ daemon runs above kernel, for MAC/Windows uses docker-machine for special VM to run docker engine on`
## Docker Commands:
```
run: 
-i interactive 
-t have basic tty 
-d detached/background 
--rm remove_after_complete 
--name  name
-p host_p:img_p
--link container_name
```
`ps: -a all`
`inspect <id>: `
`commit <c_id> <repo:tag>`

`exec <command>`
- Docker images, stored in repos (or locally) create containers. Images specified by repo+tag
- Layers: Things added to images & base image
- Making new images: Docker commit OR Dockerfiles
- Linking Containers:
- in /etc/hosts IP of container added
- No need to expose ports

## Docker File:
- RUN - Top writable file layer, commits new image
- CMD - Cmd to execute upon starting
- COPY - Host machine to docker img [usually to src]
- ADD - General COPY, that can unpack, download from internet
- USER - act as user [NEVER run container as root]
- WORKDIR - Set home

## Docker Compose:
- docker-compose up
- docker-compose ps
- docker-compose stop
- docker-compose start
- docker-compose rm
- docker-compose logs
- docker-compose build
- yml file

# Servers
* VPS: Linode/ Digital Ocean - shared servers
* Iaas/ Cloud: Virtualized Abstractions - AWS/ Azure
* Paas: Abstracted execution - Heroku, Beanstalk

# Basic Security
* PermitRootLogin no
* PasswordAuthentication no
* Use PAM: no
* ufw - Uncomplicated Firewall
ufw allow 22

