*12-08-2025 17:59*

### Status: 
 #sub-particle 


### Tags:[[03-Tags/Linux]],[[Learning]]


# Linux User & groups

## Users

### Creating, changing , switching users 
%%[[Linux common commands#^9bc402]]%%
To create user type 
`sudo adduser [username]`
	To see default values for adduser:  `useradd -D` or search for `/etc/default/useradd` that u can change
```
Gleck@gleck:~$ useradd -D
GROUP=100
HOME=/home
INACTIVE=-1
EXPIRE=
SHELL=/bin/sh
SKEL=/etc/skel
CREATE_MAIL_SPOOL=no
LOG_INIT=yes

```
`etc/login.defs`
```
159 # Password aging controls:
160 #
161 #       PASS_MAX_DAYS   Maximum number of days a password may be used.
162 #       PASS_MIN_DAYS   Minimum number of days allowed between password changes.
163 #       PASS_WARN_AGE   Number of days warning given before a password expires.
164 #
165 PASS_MAX_DAYS   99999
166 PASS_MIN_DAYS   0
167 PASS_WARN_AGE   7
```
to add password for user type
`sudo passwd [username]`
to switch user:
`su [username]`
to check environment options u can type:

### Local variables and environment

[[Bash config .bashrc| Read about bashrc.]]

`~/.bash_profile` login shell
`~/.bashrc` non login shell

if we change user `su test1`, the local variables will be passed from previous logged bash session, in other words , your current.
To prevent this , user `su - test1` , which will also change variables PATH , etc 


```
Gleck@gleck:~$ su test
Password: 
test@gleck:/home/Gleck$ cd ~
test@gleck:~$ pwd
/home/test
test@gleck:~$ 
```


```
Gleck@gleck:~$ pwd
/home/Gleck
Gleck@gleck:~$ su - test
Password: 
test@gleck:~$ pwd
/home/test
test@gleck:~$ 

```

**To check current shell session, check variable $0**

```
test@gleck:~$ echo $0
-bash
test@gleck:~$ 
logout
Gleck@gleck:~$ su test
Password: 
test@gleck:/home/Gleck$ echo $0
bash
```

### Su , Visudo

**Su** and **Visudo** are modifier for privileged open.

The command `sudo visudo` is used in Linux and Unix-like operating systems to safely edit the `/etc/sudoers` file. This file controls which users and groups are allowed to use the `sudo` command, and what privileges they have when doing so.
```
<username> <hostname.example.com>=(<run_as_user>:<run_as_group>) <path/to/command>

# User privilege specification
root	ALL=(ALL:ALL) ALL
Gleck	gleck=(ALL:ALL) ALL
netadmin	ALL=(root:ALL) /usr/bin/nmtui
helpdesk	ALL=(ALL:root) /usr/bin/passwd , !/usr/bin/passwd sysadmin, !/usr/bin/passwd root

```

Here's a breakdown of why `visudo` is used and its benefits:

## Groups and users
### Creating groups
To create group `groupadd -g 10201 -U it_sysadmin it` 
### /Etc/passwd || /etc/shadow

`tail -1 /etc/passwd`

```
test:x:1001:1001:,,,:/home/test:/bin/bash
user_name:pass:uid:gid:comments|info:user_directory:shell_interface
: separator
```

`UID` for `root` is always 0, for service users uids are in range <1,100>, for other  additional user in <100, 1000>. by default Users uid are 1000< 

`GID` are groups id, which are located in `/etc/group`

To get user **id** and **gid** `id [user_name]` 


Before , instead of x in `passwd` was HASH of Password, but now , due to security reasons , it is in `/etc/shadow`
here is information about hash function and  and pass date
Use `chage -l [user]` to see more info
```
test:$y$j9T$vRoZRlsCY67w7WIcbSoFR1$r2ah9CpN1Bjq1ei/8p4fbejYvXwKLTujBQFJ/8pOO3D:20299:0:99999:7:::
```

### Changing default user setting and environtment with usermod

NAME
	usermod - modify a user account
`usermod -aG hype Gleck`
`usermod -s /bin/zsh user_name` Changes default shell for *user_name*
`usermod test2 -d /home/users/test2 -m -aG users` will change test2 home directory to `/home/users/test2` and will add test2 to group users





### References:

- [[Linux permissions]]
- [[Bash config .bashrc]]
  