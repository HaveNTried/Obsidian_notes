*25-08-2025 12:31*
### Status: 

### Tags: [[Operating system]], [[theoretical knowledge]]


# Addressing Systems

## CHS - Cylinder-Head-Sector
![[Pasted image 20250825124740.png]]

Cylinder-Head-Sector (CHS) was an earlier method for addressing hard disks. Even though CHS now no longer maintains a physical relationship with the disk's actual characteristics, CHS is still used by many utilities.

The following are some of the terms used:

- Tracks: the concentric rings
- Each track is divided into multiple sectors
- Cylinder: a hard disk consists of one or more platters with a read-write head is located on each side of the platter. A vertical section on the corresponding ring across all platters and sides is called a cylinder.

Sectors can be addressed by CHS coordinates (up to 8 gigabytes, at least). CHS stands for:

- C: cylinder, the valid range is between 0 and 1023 cylinders
- H: Head (synonymous with side), the valid range is between 0 and 254 heads(formerly 0-15)
- S: Sector, the valid range is between 1 and 63 sectors

Of course, the indicated valid ranges do not reflect the physical facts. There are no hard disks with 128 platters (0-255 heads). These maximum values were once used by the BIOS to address the hard disk. The hard disk controller then converts the values to the actual characteristics internally. Today, CHS addressing is primarily used by partitioning utilities. Thereby, the C and H values start at 0; and the S value at 1. The first sector for a hard disk is thus located at CHS address 0/0/1. Each sector is therefore 512 bytes (hard disks with a sector size of 4,096 bytes have been available since 2010). Using 512 byte sectors, the maximum size of a hard disk would be 7,844 gigabytes using CHS addressing (1024\*255\*63\*512 is 8,422,686,720 bytes).

## LBA - Logical Block Addressing

![[Pasted image 20250825124805.png]]

For hard disks larger than 7,844 gigabytes, there continued to be a need to support CHS addressing for compatibility reasons, at least for booting. Modern BIOSs can switch to Logical Block Addressing (LBA) mode using the Int13h interrupt extensions. Using LBA, the hard disk is simply addressed as a single, large device, which simply counts the existing blocks starting at 0. An LBA block thus corresponds to a sector using CHS addressing.
## Master Boot Record

The first sector for a partitioned hard disk (LBA 0 or CHS 0/0/1) contains:

- the Master Boot Record ([[Linux SD (loader,BIOS,UIFI,partition)|MBR]])
- and the Primary or Master Partition Table (MPT)

### Extended Partition Table

Logical partitions are not defined on the Master Partition Table, but rather on a separate Extended Partition Table (EPT). The EPT is located in the first sector of the extended partition itself. The EPT is a secondary partition table, which uses the same format as the MPT. Like the MPT, it has space for four entries, however never uses more than two of them. One entry defines a logical partition. A second entry may define the rest of the extended partition.

Given multiple logical partitions, a linked list of multiple EPTs will be created (daisy chained).

## Alignment

Partitions normally start and stop at cylinder boundaries. For all primary partition (except for the first), the Partition Boot Record (PBR) is located at Head 0, Sector 1. This is not the case for the first primary partition, because the first sector must be reserved for the MBR & MPT. Therefore, the first partition with its PBR starts on the next track (head 1 of the first cylinder). Thus, the first primary partition starts at CHS address 0/1/1, while the other primary partitions respectively started at xxx/0/1.

Note: accordingly, sixty-two of the first sixty-three sectors (the remaining sectors that follow the MBR on side 0) are officially not used. Some boot managers will use the area, because it is not part of a partition. Logical partitions in the extend partition must also provide space for the EPT. For that reason, they follow the same scheme as the initial primary partition.






# References:

- [[File systems (theoretical)]]
- [[Linux SD (loader,BIOS,UIFI,partition)]]
  