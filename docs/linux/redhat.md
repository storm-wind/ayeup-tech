---
# REDHAT Specific

---
### Subscribe REDHAT VM to subscription
There are a few different options, but what I do is change hostname to what I want then do this,
```
subscription-manager register --force --username --auto-attach
```
This removes if already registered and resubscribes with auth attachment so you don;t have to go into UI to do it or eun from CLI.

You get an issue like **EE crypto too weak.**

does ths,
```
update-crypto-policies --show
update-crypto-policies --set DEFAULT
```
to make updated system-wide crypto policy active
```
update-crypto-policies
```
usually set to FUTURE

---
### Docker conflicts

When installing docker there are conflicts with podman and containerd
``
yum erase podman buildah
``
---
### Get detailed stats on a file
```
stat -L <absolute path to file>
```
---
### Check for updates etc and if a system reboot is required
```
dnf check-update
dnf needs-restarting -r
```
---

