*21-08-2025 14:04*
### Status: 
#sub-particle 
### Tags: [[03-Tags/Linux]],[[Learning]],[[Kernel architectures]]


# Linux Kernel Practical Application



This file itself is not static; it is generated from information from the modules themselves. To view information about a module, you can use the modinfo command, for example:

`modinfo radeon | less`

Here we see the location and name of the file. All modules have the .ko extension, and the .xz at the end means that the module is compressed. There is also the license, author, and description. And just below that, we have aliases—this is what the modules.alias file we looked at is generated from. And below that are the parameters—the functionality of the hardware at the driver level. For example, audio—will our video card work with sound through the same HDMI.

```
ls /sys/bus/pci/devices/00*
'/sys/bus/pci/devices/0000:00:00.0':
ari_enabled           config                    device           enable         link           modalias   power        rescan    subsystem         uevent
broken_parity_status  consistent_dma_mask_bits  dma_mask_bits    firmware_node  local_cpulist  msi_bus    power_state  resource  subsystem_device  vendor
class                 d3cold_allowed            driver_override  irq            local_cpus     numa_node  remove       revision  subsystem_vendor  waiting_for_supplier

'/sys/bus/pci/devices/0000:00:01.0':
ari_enabled           config                    device           enable         link           modalias   power        rescan    subsystem         uevent
broken_parity_status  consistent_dma_mask_bits  dma_mask_bits    firmware_node  local_cpulist  msi_bus    power_state  resource  subsystem_device  vendor
class                 d3cold_allowed            driver_override  irq            local_cpus     numa_node  remove       revision  subsystem_vendor  waiting_for_supplier

```

So, we recently learned about the **procfs** virtual file system, through which the kernel shows us information about processes. And for structured information about devices and drivers, there is the **sysfs** virtual file system, available in the */sys* directory:


```
ls /sys

ls /sys/bus/pci/device/00*
```


And although there are a lot of files here that can be used to view a great deal of information, it is not always convenient to sit and dig through these files. There are utilities that display the same information in a more compact and simple form.
`lscpu | lspsi | lsusb | lshw`

```
lscpu 
Architecture:             x86_64
  CPU op-mode(s):         32-bit, 64-bit
  Address sizes:          48 bits physical, 48 bits virtual
  Byte Order:             Little Endian
CPU(s):                   4
  On-line CPU(s) list:    0-3
Vendor ID:                AuthenticAMD
  Model name:             AMD Ryzen 5 6600H with Radeon Graphics
   CPU family:           25
   Model:                68
   Thread(s) per core:   1

```
To see what is happening in the kernel when the system starts up or now, for example, you have inserted a USB flash drive and want to understand whether the system sees it or not, you can use the ***dmesg*** utility:

`sudo dmesg -wH`

```
Aug21 14:28] usb 2-1: new high-speed USB device number 3 using ehci-pci
[  +0.319228] usb 2-1: New USB device found, idVendor=13fe, idProduct=4300, bcdDevice= 1.00
[  +0.000011] usb 2-1: New USB device strings: Mfr=1, Product=2, SerialNumber=3
[  +0.000004] usb 2-1: Product: USB DISK 2.0
[  +0.000004] usb 2-1: Manufacturer: Wilk
[  +0.000003] usb 2-1: SerialNumber: 07011B88ACB2FA35
[  +0.012587] usb-storage 2-1:1.0: USB Mass Storage device detected
[  +0.004492] scsi host3: usb-storage 2-1:1.0
[  +1.806168] scsi 3:0:0:0: Direct-Access     Wilk     USB DISK 2.0     PMAP PQ: 0 ANSI: 4
[  +0.001837] sd 3:0:0:0: Attached scsi generic sg2 type 0
[  +0.027364] sd 3:0:0:0: [sdb] 121145344 512-byte logical blocks: (62.0 GB/57.8 GiB)
[  +0.011199] sd 3:0:0:0: [sdb] Write Protect is off

```

Run the command, insert a USB flash drive or any other device, and you will see how the kernel recognizes the device.

---
The kernel automatically loads modules when it detects certain hardware or when it needs to work with certain software functionality. Although you won't have to work with this very often, you should have an idea of how it works and how to change it. For example, there may be a requirement for the system to ignore flash drives, even though, by default, you have inserted a flash drive and it is working.

So, to manage kernel modules, we use the utility:

`modprobe`

Let's say we want to load the *radeon* module, we write:

`sudo modprobe radeon`

We can see this using the *lsmod* utility, which shows the loaded modules:

`lsmod | grep radeon`

Some modules can work with others and can themselves be used by certain processes. For example, to unload a module from the kernel, you can use the same *modprobe* with the *-r* key:

`sudo modprobe -r radeon`

This module was not in use, so I was able to unload it easily. But there is a module, let's say *vboxguest*, which is in use and cannot be removed with `modprobe`:

`sudo modprobe -r vboxguest`

First, you need to get rid of the processes that use this module. For example, to unload the video card driver, you will first need to terminate all applications that use the graphical interface.

---
First, you need to get rid of the processes that use this module. For example, to unload the video card driver, you will need to first close all applications that use the graphical interface.

  

So, back to the topic of automatic module loading. The system has a program called **udev**, which is responsible for device management. When the kernel detects a new device, it creates an ==event that udev tracks==. **Udev** has a large number of rules:

`ls /usr/lib/udev/rules.d/`

which it applies when it sees a particular event. For example, *udev* sees in the event that a device is connected to the USB port that says it is a storage device, and *udev* has a rule that, when it sees such a device, creates a special file for it in the `/dev` directory with a certain name, let's say *sdb*. The kernel links this file to the driver that works with the hardware. In this way, our flash drive becomes a file, thanks to which we can interact with the device through this file. Similarly, you can specify in *udev* that when a USB device is detected, a special parameter is passed to it that would prohibit the device from doing anything. We will not delve into *udev* yet; for now, it is enough to understand why it is needed at all.
![[Pasted image 20250821152238.png]]
![[Pasted image 20250821152244.png]]

---

Sometimes it may be necessary for loadable modules to always load when the computer is turned on. To do this, you need to create a file in the `/etc/modules-load.d/` directory with .conf at the end of the name, for example, `radeon.conf`:

`sudo nano /etc/modules-load.d/radeon.conf`

where we enter the name of the module that we would like to load into the kernel when the computer is turned on. Now, after rebooting, this module will load automatically.

If we need a specific module not to load at startup or to load with certain parameters—the parameters we saw with *modinfo radeon*—then we need to create a file in the `/etc/modprobe.d` directory, also ending in *.conf*, for example, *kvm.conf*:

`sudo nano /etc/modprobe.d/kvm.conf`

Here we have some commented examples. You can read more about this on [archwiki](https://wiki.archlinux.org/index.php/Kernel_module_\(%D0%A0%D1%83%D1%81%D1%81%D0%BA%D0%B8%D0%B9\)).
# References:

- [[Linux Kernel]]
  