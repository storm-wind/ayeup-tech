# firewall-cmd

This is the firewall for yum/dnf based distros. Can be installed elsewhere, but examples here are based on RHEL7/8

### Allow multiple ports

```
for i in { 8300 8301 8302 8400 8500 8600 }
do
  firewall-cmd --zone=public --add-port=${i}/tcp
done

firewall-cmd --reload

```

### Allow a service

```
firewall-cmd --permanent --add-service https
firewall-cmd --reload
```

This adds the ports persistantly, note the **--permanent** which does this.

---

### Block outgoing port on centos/rhel
Examples of how to block some outgoing ports
```
firewall-cmd --permanent --direct --add-rule ipv4 filter OUTPUT 0 -p tcp --dport=5225 -j DROP
firewall-cmd --reload
```

To remove it
```
firewall-cmd --permanent --direct --remove-rule ipv4 filter OUTPUT 0 -p tcp --dport=5225 -j DROP
firewall-cmd --reload
```

---

