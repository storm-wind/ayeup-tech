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
# LXC Containers on Proxmox

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

## Proxmox further setup

### Add a custom backup location

Located in `/etc/pve/storage.cfg'

```
dir: backup
        path /mnt/backup
        content backup
        maxfiles 7
```
