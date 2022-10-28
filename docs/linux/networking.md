### Setup Static IP on Debian 10/11

Edit this file,

```
nano /etc/network/interfaces
```

Add the following

```
iface ens18  inet static
 address <ip address>
 netmask 255.255.255.0
 gateway <gateway ip, typically router>
 dns-domain <domain name>
 dns-nameservers <ip of dns server>
```

Restart networking service
```
systemctl restart networking.service
```
***


