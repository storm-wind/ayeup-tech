# Cico EPNMN CLI Commands
---

### Managing NCS Services

Get status of main NCS services

```
ncs status
```

On reboot, sometimes these services don't startup. So run the following.

```
ncs start
```

### Perform an NCS Cleanup
This cleans up logs files, old backup files etc, good for freeing up disk space.

In exec mode, 

```
ncs cleanup
```

---
## Managing Network interfaces

###  Remove interface from HA

in EXEC mode
```
ncs ha monitor interface del GigabitEthernet 0
```

### Change IP Address 
In EXEC mode
```
show interface
```

to enter config mode
```
configure
```

To configure interface and change IP
```
interface GigabitEthernet 0
```

This will put youn into the interface sub-menu.
```
ip address <ip address> <subnet>
```

You'll be asked to restart interface.

Then exit out of **configure** and savin g config by doing the following,
```
write memory
```

Then check it by running the following,
```
show startup-config
```
---

### Change time zone

Display time zone in EXEC mode,
```
show timezone
```

Then enter config mode and change by doing this,
```
configure
clock timezone <timezone>
exit
```
then save config
```
write memory
```


