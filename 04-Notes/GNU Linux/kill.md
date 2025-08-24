*12-08-2025 17:30*

### Status: 
#sub-particle 


### Tags: 



# kill

  ## NAME

	   kill - send a signal to a process
`kill -[options] <pid> [...]`

```
0 - ? 
1 - SIGHUP - ?, controlling terminal closed, 
2 - SIGINT - interupt process stream, ctrl-C 
3 - SIGQUIT - like ctrl-C but with a core dump, interuption by error in code, ctl-/ 
4 - SIGILL 
5 - SIGTRAP 
6 - SIGABRT 
7 - SIGBUS 
8 - SIGFPE 
 9 - SIGKILL - terminate immediately/hard kill, use when 15 doesn't work or when something disastrous might happen if the process is allowed to cont., kill -9 
10 - SIGUSR1 
11 - SIGSEGV 
12 - SIGUSR2
13 - SIGPIPE 
14 - SIGALRM
 15 - SIGTERM - terminate whenever/soft kill, typically sends SIGHUP as well? 
16 - SIGSTKFLT 
17 - SIGCHLD 
 18 - SIGCONT - Resume process, ctrl-Z (2nd)
 19 - SIGSTOP - Pause the process / free command line, 
20 - SIGTSTP 
21 - SIGTTIN 
22 - SIGTTOU
23 - SIGURG
24 - SIGXCPU
25 - SIGXFSZ
26 - SIGVTALRM
27 - SIGPROF
28 - SIGWINCH
29 - SIGIO 
29 - SIGPOLL 
30 - SIGPWR - shutdown, typically from unusual hardware failure 
31 - SIGSYS
```




### References:

- [[Linux working with processes]]
- [[Linux commands]]