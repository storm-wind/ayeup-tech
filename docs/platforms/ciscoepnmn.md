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

