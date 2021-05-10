
# Most common CLI commands for VMWare ESXi. 
Use SSH or esxi shells.

* * *
```
vim-cmd vmsvc/getallvms
```
List all VMs running on the host. Also provides vmid, required for commands below.

* * *
```
vim-cmd vmsvc/power.off vmid
```
Power off specified VM.

* * *
```
vim-cmd vmsvc/power.on vmid
```
Power off specified VM.

* * *
```
vim-cmd vmsvc/power.reboot vmid
```
Reboot specified VM.

* * *
```
vim-cmd solo/registervm /vmfs/volume/datastore/subdir/vm-file.vmx
```
Register the VM stored at location on the ESX host inventory.

* * *
```
vim-cmd vmsvc/unregister vmid
```
Unregister VM from the host. Does not remove the VM's files from the datastore.

* * *
```
vim-cmd vmsvc/destroy vmid
```
Delete the specified VM. The VMDK and VMX files will be deleted from storage as well.

* * *
```
vim-cmd vmsvc/tools.install vmid	
```
Initiates an installation of VMWare Tools on the VM

* * *
```
vim-cmd hostsvc/maintenance_mode_enter
```
Put the host into maintenance mode.

* * *
```
vim-cmd hostsvc/maintenance_mode_exit
```
Take the host out of maintenance mode.

* * *
```
vim-cmd hostsvc/net/info
```
Show networking information of the host.

* * *
```
chkconfig -l
```
Show services running on the host. Can also be used to change startup configuration.

* * *
```
esxtop	
```
Display list of processes and its usage of resources. Works similar to linux top.

* * *

```
esxcfg-info	
```
Show host's configuration and information.

* * *
```
esxcfg-nics -l
```
Show current NIC configuration.

* * *
```
esxcfg-vswitch -l	
```
Show current vSwitch configuration.

* * *
```
vmkerrcode -l	
```
Display a reference list of VMKernel return codes and descriptions.

* * *
```
dcui
```
Start the console UI (when accessing through SSH).

* * *
```
vsish	
```
Run the VMWare Interactive Shell (from SSH).

* * *
```
decodeSel /var/log/ipmi_sel.raw
```
Read IPMI system log of physical server.
Replace vmid with the value you retrieved from the getallvms command (first one in the table).Replace path with the full path and file name of a VMX-file (= configuration file of a VM).
