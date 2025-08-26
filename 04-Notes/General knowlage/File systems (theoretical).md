*24-08-2025 17:44*
### Status: 
Finished Working Frozen
### Tags: [[General knowledge]], [[Operating system]], [[theoretical knowledge]]


# file systems (theoretical)

### What is a File System?

A file system is a software component, usually implemented as a **kernel module**, that controls how data is stored, retrieved, and managed on a storage device like a hard drive or SSD. It's essentially the "librarian" for your files, keeping track of their locations, properties (metadata), and organization into a logical directory structure.

### Blocks and Fragmentation

To manage files efficiently, file systems divide the storage medium into fixed-size units called **blocks**. These are logical divisions that group together multiple physical sectors. This approach has a significant advantage: it makes it easier to track and manage files, especially large ones. For example, instead of tracking tens of thousands of individual 512-byte sectors for a 5MB file, the file system only needs to track a few dozen 4KB blocks. This helps to reduce **fragmentation**, a problem where a single file is broken into many non-contiguous pieces. While blocks are more efficient, they can lead to some wasted space if a small file doesn't completely fill a block. For example, a 1-byte file would still occupy an entire 4KB block.

---

### Inodes and Metadata

**Inodes** are a core concept in many Unix-like file systems (like ext4). An inode is a data structure that stores all the metadata about a file, such as its permissions, owner, group, size, creation date, and the addresses of the data blocks that make up the file. The file's name itself is not stored in the inode; it's stored in a separate directory entry that links the file name to its inode number. On file systems like **ext4**, the number of available inodes is determined when the file system is created. This means you could theoretically run out of inodes even if you have plenty of free disk space, a scenario that can occur if you have millions of very small files. **XFS**, a more modern file system, addresses this by creating inodes dynamically as needed.

---

### Journaling

**Journaling** is a critical feature of modern file systems that protects against data corruption. Before the file system writes data to its final location on the disk, it first records the intended operation in a special log file called a "journal." This two-step process ensures that if a power outage or system crash occurs during a write operation, the file system can use the journal upon reboot to either complete the interrupted task or undo it completely, thereby maintaining data integrity. Without journaling, an abrupt shutdown could leave the file system in an inconsistent state, leading to lost or corrupted files.

### File System Types and Linux

Linux supports many different file systems through **kernel modules**. The most common for Linux are **ext4** and **xfs**. Both have long histories and are known for their stability and performance. While Linux can read and write to file systems like NTFS and FAT32, it's not possible to install a Linux operating system on an NTFS partition because NTFS permissions and metadata structures are fundamentally different from those required by a Unix-like OS. The choice of a file system often depends on the specific use case, with ext4 being a solid default for most desktop users and xfs often being favored in enterprise environments and for large-scale file servers.









# References:

-  [[Linux working with file system Practical]]
  