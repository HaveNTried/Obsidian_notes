*12-08-2025 17:06*

### Status: 
#sub-particle

### Tags: [[Linux]],[[Learning]], [[theoretical knowledge]]



# Linux Kernel

**Linux Kernel** is a core program that ==is responsible== for:
- time sharing
- process scheduling
- memory management
- device interaction
- file system operations (like ACL checks)

![[Linux_kernel_ubiquity.svg.png]]

**Kernel is located in /boot directory with name vmlinuz**
```
root@gleck:/boot# ls -l /boot
total 191520
-rw-r--r-- 1 root root   296154 Jul  7 14:27 config-6.14.0-24-generic
-rw-r--r-- 1 root root   296154 Jul 22 15:49 config-6.14.0-27-generic
drwxr-xr-x 5 root root     4096 Jul 30 21:05 grub
lrwxrwxrwx 1 root root       28 Jul 30 21:04 initrd.img -> initrd.img-6.14.0-27-generic
-rw-r--r-- 1 root root 72777284 Jul 25 14:35 initrd.img-6.14.0-24-generic
-rw-r--r-- 1 root root 72765842 Aug 11 19:17 initrd.img-6.14.0-27-generic
lrwxrwxrwx 1 root root       28 Jul 17 20:23 initrd.img.old -> initrd.img-6.14.0-24-generic
-rw-r--r-- 1 root root   142796 Apr  8  2024 memtest86+ia32.bin
-rw-r--r-- 1 root root   143872 Apr  8  2024 memtest86+ia32.efi
-rw-r--r-- 1 root root   147744 Apr  8  2024 memtest86+x64.bin
-rw-r--r-- 1 root root   148992 Apr  8  2024 memtest86+x64.efi
-rw------- 1 root root  9145157 Jul  7 14:27 System.map-6.14.0-24-generic
-rw------- 1 root root  9145157 Jul 22 15:49 System.map-6.14.0-27-generic
lrwxrwxrwx 1 root root       25 Jul 30 21:04 vmlinuz -> vmlinuz-6.14.0-27-generic
-rw------- 1 root root 15538568 Jul  7 14:32 vmlinuz-6.14.0-24-generic
-rw------- 1 root root 15546760 Jul 22 15:53 vmlinuz-6.14.0-27-generic
lrwxrwxrwx 1 root root       25 Jul 17 20:23 vmlinuz.old -> vmlinuz-6.14.0-24-generic

```

The kernel program is  compressed and vmlinuz `z` indicates this. Whole programs size is 15Mb. When kernel needs to work with other hard-wares as network adapter etc, it calls modules. Modules have hardware drivers and instructions.The Linux kernel ==module architecture allows for dynamic loading and unloading of code into the running kernel, extending its functionality== without requiring a full kernel recompile and reboot. Linux has a ==monolithic architecture and is called monolithic kernel==. As i understood , monolithic architecture means that all processes kernel is opening in one **memory address space**.
![[OS-structure2.svg.png]]

Modules are located in `/lib/modules/$(uname -r)` kernel version directory
```
root@gleck:/lib/modules/6.14.0-27-generic# ls
build          modules.alias.bin          modules.builtin.modinfo  modules.order        vdso
initrd         modules.builtin            modules.dep              modules.softdep
kernel         modules.builtin.alias.bin  modules.dep.bin          modules.symbols
modules.alias  modules.builtin.bin        modules.devname          modules.symbols.bin
```
In *modules.alias* contains info which drivers need to be uploaded to which hardware component 
```
# Aliases extracted from modules themselves.
    2 alias cpu:type:x86,ven0000fam0006mod00BD:feature:* intel_cstate
    3 alias cpu:type:x86,ven0000fam0006mod00B5:feature:* intel_cstate
    4 alias cpu:type:x86,ven0000fam0006mod00C5:feature:* intel_cstate
    5 alias cpu:type:x86,ven0000fam0006mod00C6:feature:* intel_cstate
```
Unlike on Windows where you need to download drivers after system setup, most ==drivers on Linux are preinstalled== like modules and almost all of them are open sours.   

### Proprietary drivers

Some vendors dont like to share their source code. Usually , that are net interface developers as also invidia drivers. As they are source closed , these ==drivers are not included by default==, but ==vendor usually also provides drivers like modules==.
As i wrote before , on the alias list there are drivers that even are not used by our system like radeonfb drivers.
```
 1667 alias pci:v00001002d00005145sv*sd*bc*sc*i* radeonfb
 1668 alias pci:v00001002d00005144sv*sd*bc*sc*i* radeonfb
 1669 alias pci:v00001002d00005D57sv*sd*bc*sc*i* radeonfb
 1670 alias pci:v00001002d00005554sv*sd*bc*sc*i* radeonfb
 1671 alias pci:v00001002d00005552sv*sd*bc*sc*i* radeonfb
 1672 alias pci:v00001002d00005551sv*sd*bc*sc*i* radeonfb
 1673 alias pci:v00001002d0000554Bsv*sd*bc*sc*i* radeonfb
```
	Also we can check manufacture ready drivers on site device hunt
[sitelink](https://devicehunt.com/)  
hardware id can be found on second column , for example `pci:v00001002d00005D57sv*sd*bc*sc*i*` where:
	1002 - vendor id
	 5D57 - device id
![[dh.png]]
### modinfo
To get more info about particular driver also with module location, lincense etc,
type `modeinfo rapl` to see info about rapl driver
```
Gleck@gleck:/proc/2160$ modinfo rapl
filename:       /lib/modules/6.14.0-27-generic/kernel/arch/x86/events/rapl.ko.zst
license:        GPL
description:    Support Intel/AMD RAPL energy consumption counters
srcversion:     575437A12A7385B0072A139
alias:          cpu:type:x86,ven0000fam0006mod00BD:feature:*
alias:          cpu:type:x86,ven0000fam0006mod00B5:feature:*
alias:          cpu:type:x86,ven0000fam0006mod00C6:feature:*
alias:          cpu:type:x86,ven0000fam0006mod00C5:feature:*
...
```
### References:

-  [[Linux Kernel Practical Application]]
  