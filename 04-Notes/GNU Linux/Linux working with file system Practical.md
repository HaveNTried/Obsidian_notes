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



# References:

- [[File systems (theoretical)]]
- [[Linux working with files]]
- [[Linux working with SD(Storage device)]]