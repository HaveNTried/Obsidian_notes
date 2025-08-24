*12-08-2025 17:51*

### Status: 
#sub-particle 


### Tags: [[03-Tags/Linux]],[[Learning]]



# Linux permissions
In the topic Linux working with files we got acquainted with such a concept as inode - it stores all kinds of information about a file. With the help of stat utility we can see some of this information and now we are interested in line 4, where the standard UNIX rights to this file are specified
```
helpdesk@gleck:~$ mkdir temp
helpdesk@gleck:~$ cd temp
helpdesk@gleck:/home/it/helpdesk/temp$ touch file.txt
helpdesk@gleck:/home/it/helpdesk/temp$ {{{{{{{stat}}}}}}} file
stat: cannot statx 'file': No such file or directory
helpdesk@gleck:/home/it/helpdesk/temp$ stat file.txt
  File: file.txt
  Size: 0         	Blocks: 0          IO Block: 4096   regular empty file
Device: 8,2	Inode: 3014726     Links: 1
Access: (0644/-rw-r--r--)  Uid: (10201/helpdesk)   Gid: (10000/      IT)
Access: 2025-08-06 10:22:07.411083754 +0000
Modify: 2025-08-06 10:22:07.411083754 +0000
Change: 2025-08-06 10:22:07.411083754 +0000
 Birth: 2025-08-06 10:22:07.411083754 +0000

```
Permissions are separated among three groups:
- user(in other words owner) *u*
- group *g*
- other *o*

To check permissions also use ls - l || ll when 

```
Gleck@gleck:~$ ls -la
total 120
permissions links owner group size_in_bites
drwxr-x--- 20 Gleck Gleck  4096 Aug  7 09:33  .
drwxr-xr-x  4 root  root   4096 Aug  3 13:11  ..
-rw-------  1 Gleck Gleck  5445 Aug  7 09:22  .bash_history
-rw-r--r--  1 Gleck Gleck   220 Mar 31  2024  .bash_logout
-rw-rw-r--  1 Gleck Gleck    19 Jul 30 19:49  .bash_profile
-rw-r--r--  1 Gleck Gleck  3874 Jul 30 19:49  .bashrc
drwx------ 16 Gleck Gleck  4096 Jul 30 12:55  .cache
drwx------ 20 Gleck Gleck  4096 Aug  4 10:45  .config
drwxr-xr-x  2 Gleck Gleck  4096 Jul 31 21:15  Desktop
drwxr-xr-x  2 Gleck Gleck  4096 Jul 17 22:12  Documents
drwxr-xr-x  2 Gleck Gleck  4096 Jul 18 16:45  Downloads
drwxrwxr-x  3 Gleck Gleck  4096 Jul 19 10:53  Gitrep
drwxrwxr-x  5 Gleck Gleck  4096 Jul 19 14:02 'GleckoPitek NOtes'
-rw-rw-r--  1 Gleck Gleck 10391 Jul 21 10:09  head
-rw-------  1 Gleck Gleck    59 Aug  6 10:52  .lesshst

```

type of permissions:
r-read
w-write
x-execute

But also can be showed in numeric way , were:
r-4
w-2
x-1
## Owner change
To change owner , you need to *execute command with root rights* and to change owner **to all links in directory -R**
`sudo chown -R owner file/dir`

```
Gleck@gleck:~$ ls -l Documents/
total 0
-rw-rw-r-- 1 Gleck Gleck 0 Aug  7 10:02 some_files1.md
-rw-rw-r-- 1 Gleck Gleck 0 Aug  7 10:02 some_files2.md
-rw-rw-r-- 1 Gleck Gleck 0 Aug  7 10:02 some_files3.md
-rw-rw-r-- 1 Gleck Gleck 0 Aug  7 10:02 some_files4.md
Gleck@gleck:~$ sudo chown -R helpdesk Documents/
[sudo] password for Gleck: 
Gleck@gleck:~$ ls -l Documents/
total 0
-rw-rw-r-- 1 helpdesk Gleck 0 Aug  7 10:02 some_files1.md
-rw-rw-r-- 1 helpdesk Gleck 0 Aug  7 10:02 some_files2.md
-rw-rw-r-- 1 helpdesk Gleck 0 Aug  7 10:02 some_files3.md
-rw-rw-r-- 1 helpdesk Gleck 0 Aug  7 10:02 some_files4.md
```

## Permission change

> chmod (u/g/o)(=/-/+)(r/w/x) 

`chmod u-w file1.md`
`chmod g+x file2.md`
`chmod =rwx file3.md`

#### Directories diffs

Without execute **x** permission you cant get to hard links in directory. 

```
chmod 400 dir1
Gleck@gleck:~$ chmod 600 dir2
Gleck@gleck:~$ chmod 700 dir3
Gleck@gleck:~$ ll -r dir*
dir3:
total 12
drwx------  2 Gleck Gleck 4096 Aug  7 10:53 file1.md/
drwxr-x--- 23 Gleck Gleck 4096 Aug  7 10:53 ../
drwx------  3 Gleck Gleck 4096 Aug  7 10:53 ./

dir2:
ls: cannot access 'dir2/..': Permission denied
ls: cannot access 'dir2/.': Permission denied
ls: cannot access 'dir2/file1.md': Permission denied
total 0
d????????? ? ? ? ?            ? file1.md/
d????????? ? ? ? ?            ? ../
d????????? ? ? ? ?            ? ./

dir1:
ls: cannot access 'dir1/..': Permission denied
ls: cannot access 'dir1/.': Permission denied
ls: cannot access 'dir1/file1.md': Permission denied
total 0
d????????? ? ? ? ?            ? file1.md/
d????????? ? ? ? ?            ? ../
d????????? ? ? ? ?            ? ./

```

## sticky bit

In  situation when whole group of users have a access to shared directory with 7p, users can delete files of other users. Because directory is a list of inode links.
```
elpdesk@gleck:/home/shared$ ll
total 16
drwxrwx--- 4 root     hype 4096 Aug  7 18:08 ./
drwxr-xr-x 5 root     root 4096 Aug  7 16:56 ../
drwxr-xr-x 2 helpdesk IT   4096 Aug  7 17:28 dirTest/
drwxr-xr-x 2 Gleck    hype 4096 Aug  7 17:46 dirTest2/
-rw-r--r-- 1 Gleck    hype    0 Aug  7 17:29 yoSobaki1.txt
-rw-r--r-- 1 Gleck    hype    0 Aug  7 17:29 yoSobaki2.txt
-rw-r--r-- 1 Gleck    hype    0 Aug  7 17:29 yoSobaki3.txt
helpdesk@gleck:/home/shared$ rm yoSobaki1.txt
rm: remove write-protected regular empty file 'yoSobaki1.txt'? Y
helpdesk@gleck:/home/shared$ ll
total 16
drwxrwx--- 4 root     hype 4096 Aug  7 18:10 ./
drwxr-xr-x 5 root     root 4096 Aug  7 16:56 ../
drwxr-xr-x 2 helpdesk IT   4096 Aug  7 17:28 dirTest/
drwxr-xr-x 2 Gleck    hype 4096 Aug  7 17:46 dirTest2/
-rw-r--r-- 1 Gleck    hype    0 Aug  7 17:29 yoSobaki2.txt
-rw-r--r-- 1 Gleck    hype    0 Aug  7 17:29 yoSobaki3.txt

```

To prevent this , you can add **sticky bit** parameter using **t** key-character , or 1 on parent directory.or files—and subtract the default permissions we want. Let's say we want all new files to ha

```
Gleck@gleck:/home/shared$ sudo chmod 1770 .
Gleck@gleck:/home/shared$ sudo su helpdesk 
helpdesk@gleck:/home/shared$ rm yoSobaki2.txt 
rm: remove write-protected regular empty file 'yoSobaki2.txt'? Y
rm: cannot remove 'yoSobaki2.txt': Operation not permitted
helpdesk@gleck:/home/shared$ ll 
total 16
drwxrwx--T 4 root     hype 4096 Aug  7 18:10 ./
drwxr-xr-x 5 root     root 4096 Aug  7 16:56 ../
drwxr-xr-x 2 helpdesk IT   4096 Aug  7 17:28 dirTest/
drwxr-xr-x 2 Gleck    hype 4096 Aug  7 17:46 dirTest2/
-rw-r--r-- 1 Gleck    hype    0 Aug  7 17:29 yoSobaki2.txt
-rw-r--r-- 1 Gleck    hype    0 Aug  7 17:29 yoSobaki3.txt
```

on directory */tmp/* is also setted sticky bit   

## Setuid & Setgid
This one allows you to  run programs as owner or group member . Good example is `/usr/bin/passwd`
As you can see, instead of x, the owner has s – this means that there is a setuid attribute here, plus ls highlights this file in bright red. We know that passwd changes the user's password. The user runs the passwd program, enters the password, the program hashes the password and writes it to /etc/shadow. But a regular user does not have the rights to edit the /etc/shadow file. And the passwd process, launched on behalf of a regular user, simply would not be able to edit this file — which means the password would not be changed. That is why **setuid** is used here — when we launch the passwd program, the process is not launched on behalf of our user:
```
-rwsr-xr-x 1 root root 64152 May 30  2024 /usr/bin/passwd*
```
**Setuid** - 4
**Setgid** - 2
`chmod 4777 file1` - will set setuid 
```
-rw-r--r-- 1 root     root    0 Aug  9 13:59 file2.md
Gleck@gleck:/home/shared$ sudo chmod 6777 file2.md 
Gleck@gleck:/home/shared$ ll
total 20
drwxrwx--T 4 root     hype 4096 Aug  9 13:59 ./
drwxr-xr-x 5 root     root 4096 Aug  7 16:56 ../
drwxr-xr-x 2 helpdesk IT   4096 Aug  7 17:28 dirTest/
drwxr-xr-x 2 Gleck    hype 4096 Aug  7 17:46 dirTest2/
-rwsrwsrwx 1 root     root    0 Aug  9 13:59 file2.md*

```

**Setgid** is used to force user to make file and directories under group name
## umask
Another point – as you may have noticed, all the files I create have the same standard permissions – 664. The default permissions for new files are set by the umask utility. If you simply run umask, you will see 0002. You can also run:

```
umask -S
```

to make it clearer. So, the first digit is for sticky bit, setuid, and setgid, and the rest is for permissions. The idea is to take the maximum value—777 for directories and 666 for files—and subtract the default permissions we want. Let's say we want all new files to have 664 permissions, so we subtract 664 from 666 and get 002. Now we have 002. And if we subtract 002 from 777, we get 775.

You might say that the maximum permissions for files are also 777. But you simply cannot create new files with execute permissions, as this poses a major security threat. Therefore, the default maximum permissions for files are 666. And you can't work normally with directories without execute permissions, so they have 777. If I want files to be created with 660 permissions, I subtract 660 from 666 and get 006. But if you subtract 006 from 777, you get 771, and the 1 at the end seems pointless, so it's better to use 007 — then the permissions for directories will be 770, and for files 660.

To change this **umask** by default value , you need to change Bash config .bashrc|~/bashrc or ~/.bash_profile

## Getfacl || setfacl

To give more specific orders about permissions we can set different ALC parramms using 
	adding rule: `setfacl -m u:usertest:rwx dirtest`
	 deleting rules `setfacl -x usertest dirtest`









### References:

- [[Linux working with files]]
- 
  