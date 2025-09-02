*12-08-2025 17:35*

### Status: 
 


### Tags: [[03-Tags/Linux]],[[Learning]], [[General knowledge]]



# Linux bash


Bash, short for Bourne Again Shell, is ==a command-line interpreter and scripting language widely used in [[GNU Linux]] and Unix-like systems==. It's the default shell for most Linux distributions, providing a way for users to interact with the operating system and automate tasks through scripts,also using  

To open Bash terminal press `Shift+ Alt + T` ,but be aware ,that by default is uses current **Non ROOT user** .

You can use bash script via **[[Python]]** . [Here is a guide for this](https://www.codementor.io/@chirilovadrian360/using-bash-and-python-together-with-samples-28jpip8vmu)
But don't forget about `#!/bin/bash’`

# Linux bash commands build

***Commands*** in terminal contain: 
- *command name*-> `cp`
- *option*(arguments) ->`-r, -vi`
- *command operands* ->`file.exp , dir_exp`
***Example*** `cp -v file{1..5} test`

## Common arguments
- `-r` -> recursive
- `-v` -> verbose (shows command actions in bash)
- `-f` -> force
- `-i` -> interactive (asks for confirmation of action if necessary)

# Linux bash programs interaction

**Basically** , Unix systems have a 4 concepts :
1. Make each program do one thing well. To do a new job, build afresh rather than complicate old programs by adding new "features".
2. Expect the output of every program to become the input to another, as yet unknown, program. Don't clutter output with extraneous information. Avoid stringently columnar or binary input formats. Don't insist on interactive input.
3. Design and build software, even operating systems, to be tried early, ideally within weeks. Don't hesitate to throw away the clumsy parts and rebuild them.
4. Use tools in preference to unskilled help to lighten a programming task, even if you have to detour to build the tools and expect to throw some of them out after you've finished using them.

 So , in order to combine to simple tusk programs , we need to understand the concept of:
 1. standard input -> `stdin` **<** 
 2. standard output ->`stdout` **>**
 3. standard errorHandling -> `stderr` **2>**
	 **< (overwrite)**, **<< (add, concatenate)**
	stdout >> stdin -> |
	`grep -lr user /etc/ 2> /dev/null | tail -10`
   `{command return} ->{deleting errors}->stdout to stdin` 

```
cat /etc/passwd | head -25 | tail -20 | grep data >> file_result.md
```

# Variables (local and environments )
Variables in Linux bash system as also in other are divided between **local** and global(in this case **environments**)   
## Local
*Initialization of local variable:*


```
variable=value|"text"|$variable1
echo $variable
```

## Global

*to show all global variables type:*
`env`
```
Gleck@gleck:~$ env
SHELL=/bin/bash
SESSION_MANAGER=local/gleck:@/tmp/.ICE-unix/1997,unix/gleck:/tmp/.ICE-unix/1997
QT_ACCESSIBILITY=1
COLORTERM=truecolor
XDG_CONFIG_DIRS=/etc/xdg/xdg-ubuntu:/etc/xdg
XDG_MENU_PREFIX=gnome-
GNOME_DESKTOP_SESSION_ID=this-is-deprecated
GNOME_SHELL_SESSION_MODE=ubuntu
SSH_AUTH_SOCK=/run/user/1000/keyring/ssh
MEMORY_PRESSURE_WRITE=c29tZSAyMDAwMDAgMjAwMDAwMAA=
XMODIFIERS=@im=ibus
DESKTOP_SESSION=ubuntu
GTK_MODULES=gail:atk-bridge
PWD=/home/Gleck
LOGNAME=Gleck
XDG_SESSION_DESKTOP=ubuntu
XDG_SESSION_TYPE=wayland
SYSTEMD_EXEC_PID=2038
XAUTHORITY=/run/user/1000/.mutter-Xwaylandauth.JAT992
....
```


U can change them as usually
```
LANG=uk_UK.UTF-8 
#changes language to ukrainian
```
### useful global variables

- `HOME`- Path to home direction
- `HOSTNAME`-Computer name
- `USERNAME` - Username
- `LANG` - System language
- `PATH` - registered programs to use as comands (/usr/bin)
### Variables also as aliases are located in [[Bash config .bashrc]]

There is a huge differense between both
# End this chapter







### References:

- [[Bash config .bashrc]]
- [[Linux common commands]]
  
