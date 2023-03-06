# SElinux

I hate SElinux with a passion. So many issues I've come across are because of selinux. In an attempt to learn more about this here is some useful commands
I've come across to mitigate some of the pain.

### Set individual domains to **permissive**

Here is an examlpe for the httpd domain.
```
semange permissive -a httpd_t
```
---

### Check for unconfined processes running in unconfined domains

```
ps -eZ | grep unconfined_service_t
```
---


