*28-08-2025 14:49*
### Status: 
Finished Working Frozen
### Tags: [[Linux]], [[Learning]]


# Linux LVM

![](https://basis.gnulinux.pro/ru/latest/_images/1.jpg)

### The Problem with Traditional Partitions

With standard partitioning, a partition is a single, contiguous block of space on a disk. If a partition runs out of space, it's difficult to extend it, especially if there isn't free space immediately following it. This can lead to wasted disk space on older partitions and require time-consuming data migrations, which can cause service downtime. Additionally, some applications need to store all their data in a single directory, which makes it impossible to simply mount a new partition to that directory to add more space.

---

### The Solution: LVM
![](https://basis.gnulinux.pro/ru/latest/_images/2.jpg)
![](https://basis.gnulinux.pro/ru/latest/_images/3.jpg)
![](https://basis.gnulinux.pro/ru/latest/_images/4.jpg)


LVM is a layer that exists between the physical partitions and the file system. It provides a more flexible way to manage disk space.

- **Physical Volumes (PVs):** Physical partitions or entire disks are designated as **physical volumes (PVs)**.

- **Volume Groups (VGs):** These PVs are then combined into a **volume group (VG)**, which acts as a single, large pool of storage space.

- **Logical Volumes (LVs):** From this VG, you can create **logical volumes (LVs)**. These LVs function like traditional partitions—you create a file system on them and mount them.


The main advantage of LVM is that it allows you to dynamically resize logical volumes by adding more PVs to the volume group. You can extend a logical volume and its file system without taking the service offline, which is a major benefit for business operations. While it's also possible to shrink LVs, the text notes this is a more complex process that requires shrinking the file system first.

![](https://basis.gnulinux.pro/ru/latest/_images/5.jpg)

## Making VLM

To create  Logical Volume do everything according plan:
1. create physical volume
2. create volume group in which add PV
3. separate group on different logical volumes

> Create 3 disk partitions , download lvm2 packages

```bash
sdb      8:16   0     1G  0 disk 
├─sdb1   8:17   0   200M  0 part 
└─sdb2   8:18   0   400M  0 part 
```

Physical volume create -> `pvcreate /dev/sdb1`
```bash
root@linux2:/home/gleck# pvcreate /dev/sdb1
  Physical volume "/dev/sdb1" successfully created.
```

To output pv -> `pvdisplay`

```bash
root@linux2:/home/gleck# pvdisplay 
  "/dev/sdb1" is a new physical volume of "200.00 MiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdb1
  VG Name               
  PV Size               200.00 MiB
  Allocatable           NO
...
```

As before, to create volume group -> `vgcreate vgname /dev/sdb1`

```bash
root@linux2:/home/gleck# vgcreate myvg /dev/sdb1
  Volume group "myvg" successfully created
```

To display->`vgdisplay`
```bash
root@linux2:/home/gleck# vgdisplay 
  --- Volume group ---
  VG Name               myvg
  System ID             
  Format                lvm2
  Metadata Areas        1
...
```

Creating logical volume is more flexible and has more attributes, params as  *volume group* ,*name of logical volume* `-n`  and *size* `-L` :
`lvcreate myvg -n lv1 -L 200M`
```bash
root@linux2:/home/gleck# lvcreate myvg -n mylv1 -L 200M
  Volume group "myvg" has insufficient free space (49 extents): 50 required.
```
As we see , insufficient space, because 1 block (in this case 4 MB) is used for lv metadata. We can provide free space in percentage:
`lvcreate myvg -n lv1 -l 100%FREE`
```bash
root@linux2:/home/gleck# lvcreate myvg -n lv1 -l 100%FREE
  Logical volume "lv1" created.
```

> Note , that in case you forgot any of usage examples before, use `man` documentary.

```bash
root@linux2:/home/gleck# lvdisplay 
  --- Logical volume ---
  LV Path                /dev/myvg/lv1

```
`LV Path` is were we will **mount file system**. Also `/lv1` is a soft link to `/dev/dm-*` which also are *generated an reboot* as `/dev/sd*`  

# References:

- [[Linux working with SD(Storage device)]]
- [[Linux working with file system Practical]]
  