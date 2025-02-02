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
### SSSD service is failing

SSSD service is failing with an error 'Failed to initialize credentials using keytab [MEMORY:/etc/krb5.keytab]: Preauthentication failed.'

#### Environment
Red Hat Enterprise Linux 7
Red Hat Enterprise Linux 8

#### Issue
SSSD service is failing.
RHEL system is configured as an AD client using SSSD and AD users are unable to login to the system.
/var/log/messages file is filled up with following repeated logs.
```
Mar 13 08:36:18 testserver [sssd[ldap_child[145919]]]: Failed to initialize credentials using keytab [MEMORY:/etc/krb5.keytab]: Preauthentication failed. Unable to create GSSAPI-encrypted LDAP connection. 
```

#### Resolution
Active Directory Client

RHEL 7/8

1. Take a backup of existing /etc/sssd/sssd.conf file:

```
cp /etc/sssd/sssd.conf /tmp/sssd.bak 
```
2. Then remove the system from domain using realm command as:

3. Make sure the old Keytab is deleted:

4. Join the system to AD domain again. How to join RHEL to Active Directory using realmd


#### Root Cause
The following log indicates that the /etc/krb5.keytab file has become invalid.
```
Failed to initialize credentials using keytab [MEMORY:/etc/krb5.keytab]: Preauthentication failed. Unable to create GSSAPI-encrypted LDAP connection. 
```
#### Diagnostic Steps
Get sosreport from system in question.
Check sssd.conf file.
Check error in /var/log/messages file.
