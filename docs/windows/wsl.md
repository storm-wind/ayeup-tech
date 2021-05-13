#WSL - Windows Linux Subsystem

Some tips etc on how to use Windows WSL.

---

### Stop resolv.conf updating with local DNS
If there is an issue connecting to some stuff defined in your WSL it is most likely DNS. To stop auto generation of **resolv.conf** you need to create a file **/etc/wsl.conf** and add the following,

```
[network]
generateResolvConf = false
```

Then in a windows cmd line session,

```
wsl --shutdown
```

This shuts down wsl properly and saves any changes.

---
***

### How to import and export WSL distros
Distros already installed and up to date from MS Store.

### Import a WSL distro

Open **cmd** prompt as admin.

```
wsl --list --all
```

This will list all the WSL distros on your computer.

To import,
```
wsl --import <Name of distro> <path to save the <backupname>.tar filename
```

### Export a WSL distro

Open **cmd** prompt as admin.

```
wsl --list --all
```

Pick your distro for export.

```
wsl --export <Name of distro> <name of export>.tar file to export to
```

### To Uninstall imported WSL distros

```
wsl --list --all
```

Pick your distro

```
wsl --unregister <Name of distro>
```
And thats it.

---

***
### Fix issues with Github or SSH issues

Modify the **.bashrc** file on the end with this,
```
eval $(ssh-agent -s) && ssh-add ~/.ssh/<private key name>
```

Obviously you can use an absolute path to point to wherever you store private keys you want to use when accessing remote hosts using ssh authentication with keys.

---
***
