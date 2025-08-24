*12-08-2025 23:29*

### Status: 
  


### Tags:[[03-Tags/Linux]],[[theoretical knowledge]], [[List]]



-  **/proc**:
    
    This virtual filesystem, mounted at `/proc`, provides information about running processes, system hardware, and kernel parameters. Each running process has a corresponding subdirectory within `/proc` (e.g., `/proc/<PID>`), containing files that expose details about that process.
    
- **/sys**:
    
    Mounted at `/sys`, this virtual filesystem provides a structured view of the kernel's device model. It exposes information about devices, kernel modules, file systems, and other kernel components, allowing for system configuration and device manipulation.
    
- **/dev**:
    
    This directory contains device files, which are special files that represent hardware devices. While some device files might correspond to physical devices, many are "virtual" in the sense that they are created and managed by the `udev` device manager and do not directly represent a fixed location on a physical disk. Examples include `/dev/null`, `/dev/random`, and `/dev/zero`.
    
- **/run**:
    
    This temporary filesystem stores volatile runtime data, such as process IDs (PIDs) of running services, lock files, and other transient information needed by applications. It is typically mounted as a `tmpfs` (temporary file system) in RAM.
    
- **/tmp**:
    
    This directory is used for storing temporary files created by applications and users. Its contents are generally cleared on system reboot, making it a suitable location for temporary data that does not need to persist. It is often implemented as a `tmpfs` for performance.


### References:

- [[Linux working with processes]]
- [[Linux Kernel]]
- [[Operating system]]
- [[Linux working with SD(Storage device)]]

  