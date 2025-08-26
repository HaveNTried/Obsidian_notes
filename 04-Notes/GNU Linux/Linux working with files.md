*12-08-2025 18:06*

### Status: 
  


### Tags: [[03-Tags/Linux]],[[Learning]]



# Linux working with files


### Most simplest way using terminal
[[Linux common commands#^83020d|Commands watch here]] .

## Hard and soft links

### **What are Soft Links?**

**Quick definition:** In Linux, a soft link, also known as a symbolic link, is a special sort of file that points at a different file. In [Windows vocabulary](https://www.cbtnuggets.com/blog/certifications/microsoft/windows-to-linux-making-the-switch), you could think of it like a shortcut. Because the connection is a logical one, and not a duplication, soft links can point at entire directories or link to files on remote computers. Hard links cannot do this.
`ln  -s [original filename] [link name]`
### Advantages of Soft Link

- Versatility in linking files across different localities and document systems.
- Can link directories.

### Disadvantages of Soft Link

- Slightly slower access than hard links.
- Deleting or moving the original file will cause soft links to fail.

### **What are Hard Links?**

**Quick definition:** In the Linux operating system, a hard link is equivalent to a file stored in the hard drive – and it actually references or points to a spot on a hard drive. A hard link is a mirror copy of the original file. The distinguishing characteristic of a hard link from a soft link is that deleting the original file doesn't affect a hard link, while it renders a soft link inoperable.
`ln   [original filename] [link name]`
### Advantages of Hard Link

- It makes efficient use of disc space by avoiding the unnecessary creation of record blocks.
- There is no risk of link breaking as a result of the removal of the actual file(as long as one hard hyperlink survives, the data will persist).
- The speed of Hard Links is fast.

### Limitation of Hard Link

- Cannot span several file systems.
- Directories cannot be hyperlinked.





# File names
## hidden files

To hide file name must begin with `.` Like `.directory` 

# File editors

## vi
[[vi_cheat_sheet.pdf]]

[[vi_cheat_sheet.pdf|IF not comfortable to read]]

## Nano

**File handling**

|   |   |
|---|---|
|Ctrl+S|Save current file|
|Ctrl+O|Offer to write file ("Save as")|
|Ctrl+R|Insert a file into current one|
|Ctrl+X|Close buffer, exit from nano|

  
**Editing**

|        |                                  |
| ------ | -------------------------------- |
| Ctrl+K | Cut current line into cutbuffer  |
| Alt+6  | Copy current line into cutbuffer |
| Ctrl+U | Paste contents of cutbuffer      |
| Ctrl+] | Complete current word            |
| Alt+3  | Comment/uncomment line/region    |
| Alt+U  | Undo last action                 |
| Alt+E  | Redo last undone action          |

  
**Search and replace**

|   |   |
|---|---|
|Ctrl+B|Start backward search|
|Ctrl+F|Start forward search|
|Alt+B|Find next occurrence backward|
|Alt+F|Find next occurrence forward|
|Alt+R|Start a replacing session|

  
**Deletion**

|   |   |
|---|---|
|Ctrl+H|Delete character before cursor|
|Ctrl+D|Delete character under cursor|
|Alt+Bsp|Delete word to the left|
|Ctrl+Del|Delete word to the right|
|Alt+Del|Delete current line|

  
**Operations**

|   |   |
|---|---|
|Ctrl+T|Execute some command|
|Ctrl+T Ctrl+S|Run a spell check|
|Ctrl+T Ctrl+Y|Run a syntax check|
|Ctrl+T Ctrl+O|Run a formatter|
|Tab|Indent marked region|
|Shift+Tab|Unindent marked region|
|Ctrl+J|Justify paragraph or region|
|Alt+J|Justify entire buffer|
|Alt+T|Cut until end of buffer|
|Alt+:|Start/stop recording of macro|
|Alt+;|Replay macro|
**Moving around**

|   |   |
|---|---|
|**←**|One character backward|
|**→**|One character forward|
|Ctrl+**←**|One word backward|
|Ctrl+**→**|One word forward|
|Ctrl+A|To start of line|
|Ctrl+E|To end of line|
|Ctrl+P|One line up|
|Ctrl+N|One line down|
|Ctrl+**↑**|To previous block|
|Ctrl+**↓**|To next block|
|Alt+Home|To first row in viewport|
|Alt+End|To last row in viewport|
|Ctrl+Y|One page up|
|Ctrl+V|One page down|
|Alt+\|To top of buffer|
|Alt+/|To end of buffer|

  
**Special movement**

|   |   |
|---|---|
|Alt+G|Go to specified line|
|Alt+]|Go to complementary bracket|
|Alt+**↑**|Scroll viewport up|
|Alt+**↓**|Scroll viewport down|
|Alt+<|Switch to preceding buffer|
|Alt+>|Switch to succeeding buffer|

  
**Information**

|   |   |
|---|---|
|Ctrl+C|Report cursor position|
|Alt+D|Report line/word/character counts|
|Ctrl+G|Display help text|

  
**Various**

|   |   |
|---|---|
|Alt+A|Set or unset the mark|
|Alt+V|Enter next keystroke verbatim|
|Alt+C|Turn constant position info on/off|
|Alt+N|Turn line numbers on/off|
|Alt+P|Turn visible whitespace on/off|
|Alt+S|Turn softwrapping on/off|
|Alt+X|Hide/unhide the help lines|
|Alt+Z|Hide/unhide the info bars|
|Ctrl+L|Refresh the screen|

# Executable directories bin /usr/bin

[`/bin`](http://www.pathname.com/fhs/pub/fhs-2.3.html#BINESSENTIALUSERCOMMANDBINARIES)

> contains commands that may be used by both the system administrator and by users, but which are required when no other filesystems are mounted (e.g. in single user mode). It may also contain commands which are used indirectly by scripts

[`/usr/bin/`](http://www.pathname.com/fhs/pub/fhs-2.3.html#USRBINMOSTUSERCOMMANDS)

> This is the primary directory of executable commands on the system.

essentially, `/bin` contains executables which are required by the system for emergency repairs, booting, and single user mode. `/usr/bin` contains any binaries that aren't required.

I will note, that they can be on separate disks/partitions, `/bin` must be on the same disk as `/`. `/usr/bin` can be on another disk - although note that this configuration has been kind of broken for a while (this is why e.g. .systemd warns about this configuration on boot).
# The /etc directory

The /etc maintains a lot of files. Some of them are described below. For others, you should determine which program they belong to and read the manual page for that program. Many networking configuration files are in /etc as well, and are described in the [Networking Administrators' Guide](https://tldp.org/LDP/sag/html/etc-fs.html)






### References:

- [[Linux permissions]]
- [[Linux User & groups]]
- [[Linux working with file system Practical]]
  