
# Stunnel

Stunnel is a free linux program to encrypt traffic for any application that does not have encrypted traffic. 
You basically redirect that applications traffic to the Stunnel port and it will encrypt it across the network.


##### Config

Here is an example configuration.


```
/etc/stunnel/stunnel.conf
```

```
cert = /etc/pki/tls/certs/stunnel.pem
; Allow only TLS, thus avoiding SSL
#sslVersion = all
options = NO_SSLv2
chroot = /var/lib/stunnel
setuid = nobody
setgid = nobody
pid = /stunnel.pid
socket = l:TCP_NODELAY=1
socket = r:TCP_NODELAY=1
#foreground = no
client = yes
debug = 7
output = /log/stunnel.log


[<name of app to encrypt, ie metricbeat>]
accept = localhost:5220
connect = <ip address of destination>:<port number>
TIMEOUTclose = 0
verify = 0
```

##### Example

```
[metricbeat]
accept = localhost:5220
connect = 192.168.1.10:5225
TIMEOUTclose = 0
verify = 0
```


This is slightly different to the standard config as its in **/var/lib/stunnel*** not **/var/run/stunnel**.
This is to get around the issue when server reboots the **/var/run/stunnel** chroot gets wiped.


