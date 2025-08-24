
*21-08-2025 15:29*
### Status: 
  
### Tags:[[Learning]], [[Linux]]


# Linux working with SD(Storage device)

We figured out that when **udev** sees certain devices, it creates special files for them in the */dev* directory:

`ls /dev`

through which you can interact with the devices. And while the administrator doesn't need to do anything with most devices, storage devices require constant attention. When working with disks, you need to be extremely careful because they contain data, and any errors can lead to the loss of that data. Therefore, always make backups and make sure they are in working order.
SD types:
==HDD, SSD== ->  ==SATA interface== ,
==hard drives and optical drives== -> ==IDE interface==
==pen-drive== -> ==USB interface==
==remote SD== -> ==SAN==

At the same time, despite the differences, the **SCSI** protocol is used to work with storage devices—this applies to **USB** flash drives, **SATA** disks, **data storage networks** , and much more.

```
Gleck@gleck:/boot$ ls /dev/sd*
/dev/sda  /dev/sda1  /dev/sda2
```


Therefore, most storage devices will be named sd, i.e., scsi disk:

`ls /dev/sd*`

and then each device will be assigned a letter in alphabetical order: sda, sdb, sdc, etc. CD drives will be assigned the name sr:

`ls /dev/sr*`

sr0, sr1, etc. And in the cloud, you will come across names such as vda, vdb, etc. What do all storage devices have in common? The operating system exchanges data with these devices in the form of fixed-length data blocks. The command:

`ls -l /dev/sda`

`stat /dev/sda`

will display the symbol b before the permissions, indicating that this is a block device.
[[Character Block device]]
 
```
Gleck@gleck:/boot$ ls -l /dev/sd*
brw-rw---- 1 root disk 8, 0 Aug 21 10:31 /dev/sda
brw-rw---- 1 root disk 8, 1 Aug 21 10:31 /dev/sda1
brw-rw---- 1 root disk 8, 2 Aug 21 10:31 /dev/sda2
Gleck@gleck:/boot$ stat /dev/sda
  File: /dev/sda
  Size: 0         	Blocks: 0          IO Block: 512    block special file
Device: 0,6	Inode: 349         Links: 1     Device type: 8,0
Access: (0660/brw-rw----)  Uid: (    0/    root)   Gid: (    6/    disk)
Access: 2025-08-21 10:31:48.895999726 +0000
Modify: 2025-08-21 10:31:48.740999733 +0000
Change: 2025-08-21 10:31:48.740999733 +0000
 Birth: 2025-08-21 10:31:44.325999921 +0000
```

In addition to block devices, there are character devices—these devices work with data streams rather than blocks. Let's take the mouse as an example:

`ls -l /dev/input/mouse0`
`stat /dev/input/mouse0`

```
Gleck@gleck:/boot$ stat /dev/input/mouse0
  File: /dev/input/mouse0
  Size: 0         	Blocks: 0          IO Block: 4096   character special file
Device: 0,6	Inode: 202         Links: 1     Device type: 13,32
Access: (0660/crw-rw----)  Uid: (    0/    root)   Gid: (  995/   input)
Access: 2025-08-21 10:31:48.635999737 +0000
Modify: 2025-08-21 10:31:48.635999737 +0000
Change: 2025-08-21 10:31:48.635999737 +0000
 Birth: 2025-08-21 10:31:43.681999949 +0000

```

These files are preceded by the symbol c, which stands for character device.

But, as I said, most often drives work through SCSI protocols – so, regardless of what the **udev** rules say, we don't have to guess – we can use the `lsscsi` utility to see our disks:
`lsscsi -s`
```
 lsscsi -sl
[1:0:0:0]    cd/dvd  VBOX     CD-ROM           1.0   /dev/sr0   1.07GB
  state=running queue_depth=1 scsi_level=6 type=5 device_blocked=0 timeout=30
[2:0:0:0]    disk    ATA      VBOX HARDDISK    1.0   /dev/sda   68.7GB
  state=running queue_depth=32 scsi_level=6 type=0 device_blocked=0 timeout=30

```
---
## partition table

After creating two virtual SD with VBox.

```
lsscsi -s
[1:0:0:0]    cd/dvd  VBOX     CD-ROM           1.0   /dev/sr0   1.07GB
[2:0:0:0]    disk    ATA      VBOX HARDDISK    1.0   /dev/sda   68.7GB
[3:0:0:0]    disk    ATA      VBOX HARDDISK    1.0   /dev/sdb   1.07GB
[4:0:0:0]    disk    ATA      VBOX HARDDISK    1.0   /dev/sdc   1.07GB
```

So, we have two disks – sdb and sdc. Let's create a partition table and a couple of partitions. To do this, we will use the fdisk utility:
```
sudo fdisk /dev/sdb
sudo fdisk /dev/sdc
```

to change partition mod, in fdisk lets create partition table using **g**(`GPT`) or **o**(`MBR`) param
```
Command (m for help): p
Disk /dev/sdb: 1 GiB, 1073741824 bytes, 2097152 sectors
Disk model: VBOX HARDDISK   
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: gpt
```

Creating and deleting isnt hard to do,  But the diff u need to remember is that **GPT (GUID Partition Table)** allows to make up to **128** partitions, and **MBR(Master Boot Record)** is like legacy, with only **4** partitions available , but u can create extended logical partitions.

To check all partitions 
`fdisk -l`

### Disks utility

Has a similar usage and functionality 
![[Pasted image 20250822170441.png]]

# References:

- [[Linux Kernel Practical Application]]
- [[Linux pseudo-file system]]
- [[Linux SD (loader,BIOS,UIFI,partition)]]
  