# Groups issue
1. **Current Group Membership**:
    
    - Your primary group is `Gleck` (gid=1000)
        
    - Your supplementary groups are `Gleck` and `sudo`
        
    - The `hype` group (gid=10052) is not currently active in your session, even though your user is listed in `/etc/group` as a member of `hype`
        
2. **Why `ls` failed**:
    
    - `/home/shared` has permissions `drwxrwx---` for `root:hype`
        
    - Your current session doesn't have the `hype` group active (it's not showing in your `id` output)
        
    - This is why you get "Permission denied" even though you're in the `hype` group in `/etc/group`
        
3. **Solution**:  
    You need to refresh your group membership. Here's how:
```
# Method 1: Start a new login session (best solution)
exec su - $USER

# Method 2: Use the 'newgrp' command to activate the hype group
newgrp hype

# After either method, verify with:
id
# You should now see hype in your groups list
```