# Linux Privilege Escalation

### Enumeration

#### `hostname`

Returns the hostname of the target machine

#### `uname -a`

Prints system information giving us additional information about the kernel used

#### `/proc/version`

The proc filesystem (procfs) provides information about the target system processes. May give
information on the kernel version and additional data such as whether a compiler is installed

#### `/etc/issue`

This file usually contains information about the operating system but can be customized or 
changed

#### `ps`

List running processes `ps -A` for all running processes, `ps axjf` view process tree, 
`ps aux` for all users (a) user that launched the process (u) and processes not attached to 
a terminal (x)

#### `env`

Show environment variables, PATH may have a compiler or scripting language that can be used 
for code execution or privilege escalation

#### `sudo -l`

List commands that can be run as sudo for the current user

#### `ls`

Use `ls -al` to find hidden files

#### `id`

Will provide an overwiew of the users privilege level, use `id <USER>` to specify the user

#### `/etc/passwd`

Shows users of the system. Use `cat /etc/passwd | cut -d ":" -f 1` to extract the usernames 
or `cat /etc/passwd | grep home` to find users with home directories

#### `history`

Check for commands and typos exposing usernames and passwords

#### `ifconfig`

The target system may be a pivot point to another network. Show network interfaces of the 
system and confimr network routes with `ip route`

#### `netstat`

Gather information on existing connections. 

- `-a` shows all listening ports and established connections
- `-at` or `-au` lists TCP or UDP protocols respectively
- `-l` lists ports in listening mode (add t to list ports listening using TCP protocol) 
- `-s` list network usage statistics by protocol, add t or u for TCP or UDP
- `-tp` list connections with the service name and PID information, add l for listening ports
- `-i` shows interface statistics
- `-ano` to display all sockets (a), don't resolve names (n), (o) display timers

#### `find`

Searches the system. Can generate a lot of errors so best used with `2>/dev/null`

Find files:

- `find . -name flag1.txt`: find the file named “flag1.txt” in the current directory
- `find /home -name flag1.txt`: find the file names “flag1.txt” in the /home directory
- `find / -type d -name config`: find the directory named config under “/”
- `find / -type f -perm 0777`: find files with 777 permissions
- `find / -perm a=x`: find executable files
- `find /home -user frank`: find all files for user “frank” under “/home”
- `find / -mtime 10`: find files that were modified in the last 10 days
- `find / -atime 10`: find files that were accessed in the last 10 days
- `find / -cmin -60`: find files changed within the last hour (60 minutes)
- `find / -amin -60`: find files accesses within the last hour (60 minutes)
- `find / -size 50M`: find files 50 MB in size
- `find / -size -50M`: find files less than 50 MB in size
- `find / -size +50M`: find files greater than 50 MB in size

Find world writable and executable files and folders:

- `find / -writable -type d 2>/dev/null`: Find world-writeable folders
- `find / -perm -222 -type d 2>/dev/null`: Find world-writeable folders
- `find / -perm -o w -type d 2>/dev/null`: Find world-writeable folders
- `find / -perm -o x -type d 2>/dev/null`: Find world-executable folders

Find development tools and supported languages:

- `find / -name *perl`
- `find / -name *python`
- `find / -name *gcc`

Find specific file permissions:

- `find / -perm -u=s -type f 2>/dev/null`

### Automated Enumeration Tools

[LinPeas](https://github.com/carlospolop/privilege-escalation-awesome-scripts-suite/tree/master/linPEAS)
[LinEnum](https://github.com/rebootuser/LinEnum)
[LES (Linux Exploit Suggester)](https://github.com/mzet-/linux-exploit-suggester)
[Linux Smart Enumeration](https://github.com/diego-treitos/linux-smart-enumeration)
[Linux Priv Checker](https://github.com/linted/linuxprivchecker )

### Privilege Escalation: Kernel Exploits


