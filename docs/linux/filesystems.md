# Common filesytem Operations

#### UUID and storage

Find a device UUID and use for say mounting via UUID instead of /dev/sda as an exmample. If you have externald rives, they sometimes get mapped to different /dev/sd? so this is a good way to lock it in.

```
blkid
```

Also you can use,

```
ls -ltr /dev/disk/by-uuid
```

You can also use ***lsblk*** by using this command,

```
lsblk -f
```

If you want to use ***UUID*** to m ount a drive here is a quick example,

```
UUID=90628bbd-79d0-4455-80d4-2e289f04a7c0 /mnt/tmp auto defaults 0 0
```
---
### xargs

```
find ./ -name "*~" | xargs rm
```
This finds all files in the current directory with the name ending in "~" whcih is then piped to xargs which adds each one to its own rm command so effectively passes each file it finds to xargs.

---
### sed
modifies the contents of files sending the changed file to another file if you so wish  
```
sed [options] script-text [input-file]
```
---
### expand
converts tab to spaces and **unexpand** to convert back again.
---
### jobs
cmd to run to check if there are any jobs running in the shell session.
---
### 
