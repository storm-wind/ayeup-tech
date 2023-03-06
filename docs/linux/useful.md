# Useful Commands for General Linux Use

### GZIP on the fly with tar

```
tar cvf - FILE-LIST | gzip -c > FILE.tar.gz  

```
---
### Find file count

```
find . -xdev -type f | cut -d "/" -f 2 | sort | uniq -c | sort -n

```
---
#### PS 

```
ps -w 

```
changes so the display is wide and not limited to 80 columns and not truncated 
---
#### Special Permissions

Sticky bits

```
rwxr-xr-t

```
Allows only the owner to delete the files in that directory.Protects files from been deleted by anyone who is not the owner.
Would be used for shared folders, instances like that.

Set user id's 
In conjunction with executable files, so run the program with the permissions of the file owners, NOT the permissions of whoever runs it. 

```
SUID root 
Set Group ID

```
---
#### Check for hung processes
```
strace -p <pid>

```
Checks i/o calls from application, if nothing happens it is probably hung.

---
### Shutdown remotely
```
shutdown -f -r -m <ipaddress> -t 00
```
---
### Zombie Processes
```
pstree -ch root | more 

```

Don’t use kill -9 to kill the parent process, it does not always allow enough time to kill it cleanly. Just use kill <pid> 

---
### Check Available memory on Linux
To check available memory in Linux type free -m command. free  displays  the  total  amount of free and used physical  
and swap memory in the system, as well as the buffers used by the kernel. 
  
Command: 
```
free -m 
free -h 
free -b 
free -g 

```  
Output:

``` 
             total       used       free     shared    buffers     cached 
Mem:          2025       1961         64          0        172       1035 
-/  buffers/cache:        753       1272 
Swap:         1906          0       1906 

```
  
Description: 
The -b switch displays the amount of memory in bytes 
The -k switch (set by default) displays it in kilobytes 
The -m switch displays it in megabytes 
The -g switch displays it in gigabytes. 
  
Command: 

```
vmstat 

```  

Output:

``` 
procs -----------memory---------- ---swap-- -----io---- -system-- ----cpu---- 
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa 
 0  0      0  63316 176624 1062340    0    0    25    18   39  592  5  1 94  0 

```  

Description: 
swpd: the amount of virtual memory used. 
 free: the amount of idle memory. 
 buff: the amount of memory used as buffers. 
 cache: the amount of memory used as cache.
---
### Symlinks
#### Create a Symlink
The below example is a softlink

```
ln -s <path to the file/folder to be linked> <the path of the link to be created>

```

#### Delete a Symlink
```
unlink <path-to-symlink>

```
---
### Find
#### Remove folders less than specified parameter
```
find . -mindepth 1 -maxdepth 1 -type d -exec du -ks {} + | awk '$1 <= 250' | cut -f 2- | xargs -d \\n rm -rf

```

to just see results, then remove below form command above.

```
| xargs -d \\n rm -rf

```

---


