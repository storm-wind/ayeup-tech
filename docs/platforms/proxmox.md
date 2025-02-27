# Proxmox

## ProxMox Initial setup

### Disable Commercial Repo

```
sed -i "s/^deb/\#deb/" /etc/apt/sources.list.d/pve-enterprise.list
apt-get update
```

### Add PVE Community Repo

```
echo "deb http://download.proxmox.com/debian/pve $(grep "VERSION=" /etc/os-release | sed -n 's/.*(\(.*\)).*/\1/p') pve-no-subscription" > /etc/apt/sources.list.d/pve-no-enterprise.list
apt-get update
```

### Remove nag

```
echo "DPkg::Post-Invoke { \"dpkg -V proxmox-widget-toolkit | grep -q '/proxmoxlib\.js$'; if [ \$? -eq 1 ]; then { echo 'Removing subscription nag from UI...'; sed -i '/data.status/{s/\!//;s/Active/NoMoreNagging/}' /usr/share/javascript/proxmox-widget-toolkit/proxmoxlib.js; }; fi\"; };" > /etc/apt/apt.conf.d/no-nag-script
apt --reinstall install proxmox-widget-toolkit
```

***

### Install **qemu** agent

```
apt-get install -y qemu-guest-agent

systemctl enable qemu-guest-agent

systemctl start qemu-guest-agent
```

***
### LXC Containers on Proxmox

LXC containers are containers much like Docker but unlike docker have their own namespaces and utilise the host system kernel. I use LXC for a variety of purposes.
Mainly for services I want to isolate and have several dependencies around it. For example, Pi-Hole or a reverse proxy. These can be run from Docker as well but I like the fact I can manage these LXC containers through Proxmox UI.
Anyway, some caveats I found when creating LXC containers.

- SSH is not enabled by default
- space on filesystem to create SSH session doesn't work. So heres what you do.

```
systemctl start sshd

mkdir /run/sshd
```

Thats it.

***

### Proxmox further setup

#### Add a custom backup location

Located in `/etc/pve/storage.cfg'

```
dir: backup
        path /mnt/backup
        content backup
        maxfiles 7

```

#### Install Samba for Windows Shares
Do the following to add a smba share and have R/W access.

```
apt install samba -y
adduser <username>
smdpasswd -d <username>
smbdpasswd -a <username>
```

```
nano /etc/samba/smb.conf
```
Add the following,

```
[<share label>]
comment = <description of share>
path = <location you want to share>
browseable = yes
read only = no
writable = yes
write list = <username>
valid users = <username>
guest = no
create mask = 0775
directory mask = 0775
```

```
systemctl enable smbd
systemctl restart smbd
```

***

### Run Proxmox from a Laptop

I run proxmox on an old laptop for several reasons.~

Repurposing old hardware.~
Its relatively quiet.~
Low power consumption.~
Built in UPS (battery)~
Small foot print.~
Once installed there a few extra things to consider.

#### Disable suspension on lid closure
Don't want laptop to shutdown when the lid is closed, 

Run this,

```
systemctl mask sleep.target suspend.target hibernate.target hybrid-sleep.target
```

#### Change AppArmour policy for NFS mounts in CT's

```
nano /etc/apparmor.d/lxc/lxc-default-cgns
```

Change it to this,

```
# Do not load this file.  Rather, load /etc/apparmor.d/lxc-containers, which
# will source all profiles under /etc/apparmor.d/lxc

profile lxc-container-default-cgns flags=(attach_disconnected,mediate_deleted) {
  #include <abstractions/lxc/container-base>

  # the container may never be allowed to mount devpts.  If it does, it
  # will remount the host's devpts.  We could allow it to do it with
  # the newinstance option (but, right now, we don't).
  deny mount fstype=devpts,

# Extra stuff added
  mount fstype=nfs,
  mount fstype=cgroup -> /sys/fs/cgroup/**,
  mount fstype=cgroup2 -> /sys/fs/cgroup/**,
}

```
Then reload apparmour service, 

```
service apparmor reload
```

***

### Add NFS Mount inside a Proxmox Container
Make sure the container is down.

Run the following in the main proxmox console

```
pct set 1000 -mp0 /mnt/pve/data,mp=/mnt/data
```

Lets break it down,

`pct set 1000` is the container to apply it to.

`-mp0` the mount point, if you wanted multiple you would add `-mp1` as an example.

`/mnt/pve/data` is the local mount point in proxmox not in the container.

`mp=/mnt/data` is the mount point created within the container when it starts up. Note you don't need to add anything in the `/etc/fstab` config in the container. I tried this approach and could never get it working the way I wanted.

***
### Kill a Container or VM that won't shutdown

Log into shell for proxmox host.
```
ps aux | grep VMID 
```

You'll get the pid that way then,
```
kill -9 pid
```

***
### What to do when there is no monitoring appearing in Proxmox UI Summary
```
systemctl stop rrdcached
mv /var/lib/rrdcached/db /var/lib/rrdcached/db.bck
systemctl start rrdcached
systemctl restart pve-cluster
```
Or you could just delete the sub folders like I did if you don;t want the history.

***



