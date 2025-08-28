
Initializing a GUID Partition Table (GPT) on a virtual disk in Linux with a BIOS-based system is possible because the **Linux kernel and modern partitioning tools are designed to handle both BIOS/MBR and BIOS/GPT configurations**. This is a key difference from how many older operating systems and even some versions of Windows behave.

---

### How It Works

- **Protective MBR:** A GPT-formatted disk still includes a **protective MBR** (Master Boot Record) at the very beginning. This MBR is a placeholder that prevents older, GPT-unaware tools from accidentally overwriting the GPT data.
- **Linux Partitioning Tools:** Tools like `gdisk` or `parted` in Linux can recognize and properly work with the GPT structure.
- **BIOS Boot Partition:** To boot a Linux system from a GPT disk on a BIOS machine, a small, unformatted **BIOS boot partition** (typically 1-2 MiB) is required. This is where the bootloader, such as GRUB, stores its core code because the traditional MBR gap used for this purpose isn't available on a GPT disk. The BIOS reads the protective MBR, which then directs it to the GRUB bootloader in this dedicated partition, allowing the system to boot.
# References:
[[SD (loader,BIOS,UIFI,partition)]][[Linux]]