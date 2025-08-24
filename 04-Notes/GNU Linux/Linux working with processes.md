*12-08-2025 18:07*

### Status: 
 


### Tags:[[Linux]],[[Learning]]



# Linux working with processes
# process screen
1. **Program Execution & RAM**: Programs are loaded into RAM for faster access since disks/SSDs are slower. Large programs are loaded partially, as needed.
2. **Virtual Memory**: Each program operates as if it has exclusive RAM access through virtual memory.
3. **Process Components**: A process includes the program, its dependencies (libraries, config files like `/etc/nanorc`), open files, environment variables, and CPU operations for data processing.
4. **Parallel Operations**: Programs may split tasks (e.g., complex equations) for parallel execution. Example: A web server handles multiple clients concurrently by processing requests in parallel.
In short, a **process** is a running program with all its resources, enabling efficient and isolated execution, sometimes with parallel task handling.

## ps

So, a running program is a process. To begin with, it is important for an administrator to see a list of processes. There are several ways to do this, let's start with the ps utility. If you just run: 
```
ps
```

you may notice a lot of duplicated keys. But ps documentation is huge, and you don't need to know all the keys. It is enough to learn one combination that will work in most cases:

  

```
ps -ef
```

and if you need something specific, you can always google it or look it up in man.

Here’s a structured summary of the key points about process attributes and hierarchy in Linux:

### **Process Attributes in `ps` Output**

1. **UID (User ID)**:
    
    - Identifies the user who launched the process.
        
    - **Root (superuser)**: Has full system access; most system processes run as `root`.
        
    - **Service users**: Created for programs that don’t need root privileges (e.g., `www-data` for web servers). Minimizes risk if a process is compromised.
        
    - **Regular users**: Processes like your terminal emulator run under your user ID.
        
2. **PID (Process ID)**:
    
    - Unique numerical identifier for each process.
        
    - Reused after a process terminates (not immediate).
        
    - Used to control processes (e.g., killing with [`kill PID` ](kill.md)).
        
3. **PPID (Parent Process ID)**:
    
    - Shows the PID of the process that spawned the current one.
        
    - Example:
        
        - Terminal → `bash` → `nano`:
            
            - `nano`’s PPID = `bash`’s PID.
                
            - `bash`’s PPID = terminal emulator (e.g., `gnome-terminal`).
                
    - **Init/systemd (PID 1)**: Ultimate parent of most processes.
        

### **Key Observations**

- Even a minimal system runs hundreds of processes (many as `root` or service users).
    
- Process hierarchy reflects dependency (e.g., terminal → shell → editor).
    
- Commands like `ps -ef` or `ps -f --ppid PPID` help trace parent-child relationships.
    

### **Why It Matters**

- **Security**: Least privilege principle (avoid running everything as `root`).
    
- **Linux Troubleshooting**: Identify orphaned/zombie processes or [[kill]] misbehaving ones.
    
- **System understanding**: Processes are organized in a tree structure.
    

Example command to inspect a process’s parent:

bash

```
ps -f --ppid [PPID]  # Shows children of a specific parent process
```

## Procfs
[[Linux pseudo-file system]] allows to see particular process info, u can access it via`cd /proc`.
To get info about chosen process `cat /proc/pid/status` 

## Process priority 
U can lower priorities by increasing `nice` parameter 



### References:

- [[Linux pseudo-file system]]
  