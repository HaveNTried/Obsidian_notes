*24-08-2025 17:38*
### Status: 
Finished Working Frozen
### Tags: [[Linux]], [[Learning]]


# Linux file system

Let's write the file system. The `mkfs` utility will help us with this. All default settings for the *ext4* file system are located in the `/etc/mke2fs.conf` file:

``` bash 
head /etc/mke2fs.conf
```

Here we have block sizes, inodes, and so on.
```bash
root@linux2:~# head -50 /etc/mke2fs.conf 
[defaults]
	base_features = sparse_super,large_file,filetype,resize_inode,dir_index,ext_attr
	default_mntopts = acl,user_xattr
	enable_periodic_fsck = 0
	blocksize = 4096
	inode_size = 256
	inode_ratio = 16384

[fs_types]
	ext3 = {
		features = has_journal
	}
	ext4 = {
		features = has_journal,extent,huge_file,flex_bg,metadata_csum,64bit,dir_nlink,extra_isize
	}

```

All that remains for us to do to create the file system is to write the command **mkfs dot, the desired file system type, and then the partition or disk**:

```bash
sudo mkfs.ext4 /dev/sdb1
```

The program will create the file system and show us information about it: how many blocks of what size fit in the file system, how many **inodes**, the **file system ID**, where the **superblock** backups are stored—the very one where the metadata of the file system itself is stored.
```bash
root@linux2:~# mkfs.ext4 /dev/sdb1
mke2fs 1.47.0 (5-Feb-2023)
Creating filesystem with 51200 4k blocks and 51200 inodes
Filesystem UUID: 2bde4dad-e500-4bf9-8f5c-db0c0c102929
Superblock backups stored on blocks: 
	32768

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: done
```

For additional information type `tune2fs -l` with **partion destination**
```bash
root@linux2:~# tune2fs -l /dev/sdb2
tune2fs 1.47.0 (5-Feb-2023)
tune2fs: Attempt to read block from filesystem resulted in short read while trying to open /dev/sdb2
Couldn t find valid filesystem superblock.
root@linux2:~# tune2fs -l /dev/sdb1
tune2fs 1.47.0 (5-Feb-2023)
Filesystem volume name:   <none>
Last mounted on:          <not available>
Filesystem UUID:          2bde4dad-e500-4bf9-8f5c-db0c0c102929
Filesystem magic number:  0xEF53
Filesystem revision #:    1 (dynamic)
Filesystem features:      has_journal ext_attr resize_inode dir_index filetype extent 64bit flex_bg sparse_super large_file huge_file dir_nlink extra_isize metadata_csum
Filesystem flags:         signed_directory_hash 

```

---

For most users, there's no need to manually configure a file system because the default settings work well out of the box. However, for more complex tasks, you'll need to dive into the documentation.

The text then introduces the concept of **mounting** a file system. This is the process of attaching a file system to a specific directory in your existing file system, making its contents accessible. The reasons for doing this can vary depending on your needs:

- **Separating user files:** Mounting a separate partition to the `/home` directory allows you to reinstall your operating system without wiping out all of your personal files.
- **Isolating data:** On a server, you might want to mount a separate disk for user data to keep it distinct from the operating system files.
- **Controlling log growth:** You could also mount a separate partition for your system logs to prevent them from suddenly filling up your main disk and causing the system to crash.

an example of moving user files to a new file system. The key point is that this isn't a simple drag-and-drop process. If you simply mount a new, empty file system to the `/home` directory, you'll hide all the existing user files. Instead, you must first move the old files to the new file system.

This task is complicated by the fact that many programs are constantly using files in your home directory (e.g., your browser writes cache, your terminal saves history). Moving files while they're in use could corrupt them. Therefore, you must stop all processes that are using the `/home` directory and its subdirectories before you can safely perform the migration.

>To avoid corrupting files, we need to stop all processes used by home directory. So further, we will use virtual terminal, as over GUI is stopped

### Virtual terminal

The conception is simple - we need to:
1. setup our partition table for partition we want to use
2. initialize file system
3. mount to that partition `/mnt`
4. move files from directory to `/mnt`
5. unmount file system we used
6. write rule in `/etc/fstab`
7. unmount `/mnt`
#### Preparation

We make sure that no process is using files in the /home directory.
The `lsof` utility, which shows open files, will help us with this, with the `+D` key to check all subdirectories, `du -h /home` to check `/home` size and `fdsik -l /dev/sd*` to check partition size
```bash
lsof +D /home
du -h /home
fdsik -l /dev/sd*
```

Then , initializing file system:
```bash
mkfs.ext4 /dev/sdc2
```
`mkfs` utility initializes `ext4` file system on `/dev/sdc2`.
Now I need to move the files from the `/home` directory to the new file system. To do this, I need to temporarily mount the new file system. There is even a special directory for this purpose - `/mnt`. I simply write:
```bash 
mount /dev/sdc2 /mnt
```

i.e., which file system to mount to which directory. To make sure everything worked, I use the df utility:
```bash
df -h
```
Okay, time to move the files:  

```bash
mv -v /home/* /mnt
```

Let's check if everything is okay:

```bash
ll /mnt /home
du -h /mnt | tail -1
```
Everything seems to be okay—the files have been moved. Now it's time to unmount the new file system and mount it in /home. To unmount, use:
`umount /mnt`
and to mount we need to write instruction in `/etc/fstab`
To write partition id in `/etc/fstab`:
```bash
blkid /dev/sdc2 | cut -d' ' -f2 >> /etc/fstab
```
Then , open it with **nano** editor.
Now we can return to the graphical interface – `Ctrl+F1` and log in. Let's type `df -h` again and see what is mounted where. As you can see, the `sdc2` file system is mounted in the /home directory. And if something wasn't working, we simply wouldn't be able to log in.

  

So, when the computer is turned on, the operating system looks at `fstab`, sees which file system to mount where and how, and does so. But what if the file system is unavailable? Let's say there are some problems with the disk or the file system itself, or maybe there is a typo in `fstab`. In such cases, the operating system does not boot completely, but prompts us to look at and fix the error. The operating system cannot ignore file system problems and boot if this is not specified in `fstab`. Imagine that there are some important files on this file system, such as application files. If the system starts up and launches the application, but it cannot see its files, the application may cause a lot of problems. To avoid this, the operating system will not allow the entire system to start if it **cannot mount** a file system.

---
### Bag fixing

Lets recreate a mounting bag and fix it:

```bash
sudo nano /etc/fstab
```

Before UUID we lets add some text:

```
reboot
```

![](https://basis.gnulinux.pro/ru/latest/_images/journalctlxb.png)

We see system recommends us to see log files. Enter `root` password and open logs:

```bash
journalctl -xb
```

We see red line – Failed to mount /home – that means we couldnt mount  /home. a bit higher we see that system cant find device

![](https://basis.gnulinux.pro/ru/latest/_images/fscksdc2.png)

But before fixing it, let's assume that it's not a typo on our part, but some kind of problem with the file system. To do this, we'll check the file system using the utility fsck:

```bash
fsck /dev/sdc2
```

And for the **xfs** file system, the `xfs_repair` utility is used. I don't have any errors right now, but if I did, the program would tell me about them and suggest solutions.

![](https://basis.gnulinux.pro/ru/latest/_images/ll.png)

- **General Principle:** It's often beneficial to separate certain directories from the root file system, but this isn't always necessary. The goal is often to improve security, simplify backups, or prevent one part of the system from filling up the disk and affecting another.
    
- **Specific Directories:**
    
    - **/home**: It makes sense to separate this directory to keep user data separate from the system files.
        
    - **/boot**: This directory might also be separated, though the text says this will be discussed later.
        
    - **/dev**, **/proc**, **/sys**, and **/run**: These are virtual file systems, so separating them isn't applicable.
        
    - **/etc**: This directory contains critical system configuration files. Separating it is difficult and pointless.
        
    - **/media** and **/mnt**: These are temporary mount points, so separating them is meaningless.
        
    - **/opt**: This directory often holds proprietary software. Separating it can make it easier to back up and migrate applications, especially on servers.
        
    - **/root**: The root user's home directory. It's usually not worth separating.
        
    - **/tmp**: This directory holds temporary files. The text suggests it often makes sense to mount it as a virtual file system to prevent it from growing too large.
        
    - **/usr**: This directory used to be separated to make it read-only for security reasons. However, this is no longer recommended in modern distributions, as the entire root can be made read-only under certain conditions.
        
    - **/var**: This directory contains dynamic, changing files like logs and databases. Separating it is crucial for a read-only system. For example, separating **`/var/log`** is recommended to prevent excessive log files from filling up the entire disk and causing system issues.
        
- **Practical Considerations:** The text concludes by emphasizing that while these operations may seem simple, they can be time-consuming with large amounts of data, leading to service downtime and financial loss for businesses. Therefore, system administrators must plan ahead during the initial system installation to determine partition sizes, file system block sizes, and inode counts. The text also briefly mentions that there are more flexible solutions for storage, which will be discussed later.


# References:

- [[File systems (theoretical)]]
- [[Linux working with files]]
- [[Linux working with SD(Storage device)]]