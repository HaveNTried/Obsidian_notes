*12-08-2025 17:21*

### Status: 
  


### Tags: [[03-Tags/Linux]] [[Learning]]


# Bash config .bashrc

`~/.bash_profile` Login shell
`~/.bashrc` Non-login shell

The _[.bashrc](https://phoenixnap.com/kb/bashrc)_ and _.bash_profile_ startup files help customize the [Unix](https://phoenixnap.com/glossary/what-is-unix) command-line environment. The configuration files hold useful custom information, such as PATH directories, [command aliases](https://phoenixnap.com/kb/linux-alias-command), and custom styles

``
#### bashrc

The _.bashrc_ file is a hidden script file containing various terminal session configurations. The file executes automatically in both interactive and non-interactive non-login shells.

When running a non-login shell (subshell), the primary read configuration file is the _/etc/bash.bashrc_. The file contains system-wide configurations for non-login shells.

#### .bash_profile

The _.bash_profile_ file is a hidden script file with custom configurations for a user terminal session. The file automatically executes in Bash interactive login shells.

When running an interactive login shell, the system reads the following configuration file first:

- _**/etc/profile**_ - Stores global configurations for login shells. The configurations apply to all users.

Next, the Bash shell searches for specific user configuration files in the following order:

- **_~/.bash_profile_**
- **_~/.bash_login_**
- **_~/.profile_**

The first found file is read and executed.
## Difference Between .bashrc and .bash_profile

The critical differences between _.bashrc_ and _.bash_profile_ are:

- _.bashrc_ defines the settings for a user when running a **subshell**. Add custom configurations to this file to make parameters available in subshells for a specific user.
- _.bash_profile_ defines the settings for a user when running a **login shell**. Add custom configurations to this file to make parameters available to a specific user when running a login shell.








### References:

- [[Linux bash]]
