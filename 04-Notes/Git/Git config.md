*24-08-2025 14:39*
### Status: 
Finished Working Frozen
### Tags:[[Git]]


# Git config

The most basic use case for `git config` is to invoke it with a configuration name, which will display the set value at that name. Configuration names are dot delimited strings composed of a 'section' and a 'key' based on their hierarchy. For example: `user.email`

```bash
git config user.email
```

In this example, email is a child property of the user configuration block. This will return the configured email address, if any, that Git will associate with locally created commits.
```bash
git config --global --list
```
To see values
### git config levels and files

Before we further discuss `git config` usage, let's take a moment to cover configuration levels. The `git config` command can accept arguments to specify which configuration level to operate on. The following configuration levels are available:
- `--local`

By default, `git config` will write to a local level if no configuration option is passed. Local level configuration is applied to the context repository `git config` gets invoked in. Local configuration values are stored in a file that can be found in the repo's .git directory: `.git/config`  

-  `--global`

Global level configuration is user-specific, meaning it is applied to an operating system user. Global configuration values are stored in a file that is located in a user's home directory. `~ /.gitconfig` on unix systems and `C:\Users\\.gitconfig` on windows  

-  `--system`

System-level configuration is applied across an entire machine. This covers all users on an operating system and all repos. The system level configuration file lives in a `gitconfig` file off the system root path. `$(prefix)/etc/gitconfig` on unix systems. On windows this file can be found at `C:\Documents and Settings\All Users\Application Data\Git\config` on Windows XP, and in `C:\ProgramData\Git\config` on Windows Vista and newer.

Thus the order of priority for configuration levels is: local, global, system. This means when looking for a configuration value, Git will start at the local level and bubble up to the system level.  

### Writing a value

Expanding on what we already know about `git config`, let's look at an example in which we write a value:

```css
git config --global user.email "your_email@example.com"
```

This example writes the value `your_email@example.com` to the configuration name `user.email`. It uses the `--global` flag so this value is set for the current operating system user.






# References:

- [[Git ignore]]
  