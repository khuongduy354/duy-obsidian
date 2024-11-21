# Overview
- Ken & Ritchie made UNIX (an operating system), Ritchie ? sounds familiar ? Yes check author in C programming language book
### stdin, out, err, pipe , and file descriptor (handling in C/C++) 
[[C++]]  [[C++ Essence#reading file]]
 https://www.codequoi.com/en/handling-a-file-by-its-descriptor-in-c/ 
 - a file descriptor can points to more than a (text or binary) file, it's an int point to some File object (see link)  
 - fd can points to pipe, input from terminal,..

- stdin is a file descriptor = 0 
``` 
ls > output.txt # directs stdout to output.txt
ls | grep something # directs stdout to stdin of grep  
``` 

### cron jobs 
```
30 08 * * * /home/pete/scripts/change_wallpaper 

unixtime   runscript
```

### packages manager
 - /etc/apt/sources.list store repos (used for searching package) of apt   1
```
# your bins get searched not just from current directory, 
# but through all your paths
env #shows environmnet variables  
$PATH #contains list of paths seperated by :  

```
# permissions 
**USERS**
``` 
users  (UID), has user private folder
groups (GID), permissions given to certain users 
sudo: superuser 

# uid used for identify users   
etc/passwd # see  username map to id, and lots of infos
etc/shadow # store encrypted password, ironic? etc/passwd doesnt store password,  
etc/groups # group data 

sudo useradd myname 
sudo userdel myname  
passwd myname #change password
```
**FILES**   
- sticky bit: allow only owner or everyone to do vodoo on files!
```
chmod u+x myfile  #add permission
sudo chown patty myfile #ownerships
sudo chgrp whales myfile
```
- apply groups and users set above 
https://linuxjourney.com/lesson/file-permissions <- very clear here  

# process   
- when a process launched it has 3 ids: https://linuxjourney.com/lesson/process-permissions
- each has a pid   
- process states: RSDZT 
- stats here: https://linuxjourney.com/lesson/tracking-processes-top 
- and here https://linuxjourney.com/lesson/cpu-monitoring
### signal   
- use for interrupts
- 9 = KILL (ctrl c emit this signal), see full list for others
 
```
ps aux 

PID        TTY     STAT   TIME          CMD
41230    pts/4    Ss        00:00:00     bash
51224    pts/4    R+        00:00:00     ps

kill pid  

ps proc # current processes infos 



fuser   # which processes using this file
lsof    # like above, but more, in a table, which processes using which files

``` 
### multithreading 
- thread = lightweight process 
- a process must have at least 1 thread  
- many threads of process shares same resource
[[multithreading]]   [[coroutine, generators, thread]]  




### background 
- just like  in vim ctrl z: send to bg, fg to get to fg 
``` 
sleep 1000 & #add & means run in bg 
jobs # list all bg jobs 
fg  
``` 

# logging 
- /var store system logs 
- syslog (or newer rsyslog) track events and decide to write into /var/log/syslog 
- /etc/rsyslog.d   see config for logging system

# drivers, devices 
/dev: store devices info 
/sys: more detailed and complex than /dev, can show manufacturers
-> it's a virtual filesystem actually (called sysfs) 

```
$ ls -l /dev
  
brw-rw----   1 root disk      8,   0 Dec 20 20:13 sda
  
crw-rw-rw-   1 root root      1,   3 Dec 20 20:13 null
  
srw-rw-rw-   1 root root           0 Dec 20 20:13 log
  
prw-r--r--   1 root root           0 Dec 20 20:13 fdata




mknod /dev/sdb1 b 8 3 # make a device 
udev # handles above command automagically
``` 
### types
- character: transfer data, but 1 char a time
- block: same above but in block
- pipe: transfer data, but output to another process, not device
- socket: same above but many processes   

- major: which driver class is used 
- minor which specific 
- e.g: sda: sd major a minor, we have sdb, sdc,...
### names  
**scsi** 
- sda : first hard disk
- sdb : second hard disk
- sda3: third partition of first hard disk  
**PATA** 
- same above but  hda hdb hda3,...
**pseudo**  
- not physically connected device
- /dev/random: generate random number

# Booting 
[[booting]]  [[kernel]]  
https://linuxjourney.com/lesson/boot-process-overview 
- BIOS -> Bootloader -> Kernel -> init 
- Check hardwares -> kernel copied to memory  ->  load essentials modules, mounting root filesystems -> start stop some processes (based on startup scripts) 
# Kernel 
1
# Filesystem   

https://linuxjourney.com/lesson/filesystem-hierarchy
- /usr /etc /var: see link 
- journal file system: write to a log before doing write operation in real filesystem   
### swap 
- disk memory but act as buffer (extra) RAM memory , use when it overloaded,  
 
```
df # list filesystems   


parted  # run program
select /dev/sdb2  # select device
print # list infos
mkpart primary 123 4567 # partition starts and endpoint (in mbs, kbs, gbs)    
resize 

# gparted is the GUI version of above  



sudo mkfs -t ext4 /dev/sdb2  # creating filesystem   

sudo mount -t ext4 /dev/sdb2 /mydrive # mount if want to be used 
$ sudo umount /mydrive 
or 
$ sudo umount /dev/sdb2 

### /etc/fstab auto mount at startup


```  

- 1 disk can have many partitions: sda is a disk and sda1 is first partition of that disk 
- implement: Partition Table MBR or GPT, track start, end of partitions...  
- MBR: logic, primary, extended 
- GPT: guid only, simpler and more modern
### components
![[Pasted image 20240127181654.png]]
### inode 
- a database stores metadata (pointer to content blocks, permissions) of file except its content and filename  
- its content is in data block of filesystem
-> just like in a backend database, a table store link to an image not the real image (the real image is stored on a Cloud Static Storage)
```
ls -li
140 drwxr-xr-x 2 pete pete 6 Jan 20 20:13 Desktop
141 drwxr-xr-x 2 pete pete 6 Jan 20 20:01 Documents 

-> see 140,141? these are inodes number, used to identify 
 
``` 
- how it pointed 
 ![[Pasted image 20240127182946.png]]
 - Ah i saw this pattern in [[git clone rust]], where blobs are hashed and store, but instead of all, we take the 3 first chars of hash, and make it a folder, this indirectness, reduce the query numbers significantly    
 ### symlinks 
```
$ ln -s myfile softlink #(symlink)
$ ln somefile hardlink 
```
 - Linux counterpart of Windows shortcut 
 -> why not use inode for shortcut:  inode unique to each filesystems, so can't reference from another filesystems with just inode (requires more context like from where, but that extra complex) 
 -> hence symlinks useful which points to a filename (not an inode) 
 ```
 ln -s myfile myfilelink  
 
 151 -rw-rw-r-- 1 pete pete 7 Jan 21 21:36 myfile
  
93403 lrwxrwxrwx 1 pete pete 6 Jan 21 21:39 myfilelink -> myfile
``` 
-> delete  myfile = myfilelink not working
### hardlink    
- a file  points to filename like symlinks but not if pointed data removed, still accessible!  
-  because it has points to same  inode, and points to filename too
```

ln myfile2 myhardlink
93401 -rw-rw-r-- 2 pete pete 8 Jan 21 21:36 myfile2
  
93403 lrwxrwxrwx 1 pete pete 6 Jan 21 21:39 myfilelink -> myfile # a hardlink (file) that has filename as content 
  
93401 -rw-rw-r-- 2 pete pete 8 Jan 21 21:36 myhardlink  # this is different from soft link, a hardlink (file) points to same inode above (which has exact same content)
``` 
-> because it has another file which has point to same inode, it still accessible after myfile2 deleted  
-> permanent delete when: myfile deleted reduce inode counts -> count == 0 -> delete inode  
-> same filesystem only
# grep fu 
grep101 online

# regex fu 
regex101 online


# Networking  
[[networking]]  
### DNS  
lookup local first: a map file in /etc/hosts     
### ssh  
can access same network remote host through ssh (scp)





# linux
--- 
# Refererences 




2024 01 26 22:38
#literature [[linux]] [[low level]] [[computer science]] [[operating system]] 