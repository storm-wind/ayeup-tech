# Join this host to the usual domain domain and add appropriate
```
yum install adcli oddjob oddjob-mkhomedir realmd samba-common samba-common-tools sssd
```
```
realm join <local domain> -U join
```
```
nano /etc/sssd/sssd.conf
# SET use_fully_qualified_names = False
# SET fallback_homedir = /home/%u

systemctl enable sssd
systemctl start sssd

cat >/etc/sudoers.d/devops <<EOF

%<group 1>              ALL=(ALL) ALL

EOF

%<group 2>            ALL=(ALL) ALL
```

```
yum -y install realmd sssd oddjob oddjob-mkhomedir adcli samba-common
```
```
realm join <local domain> -U *your admin AD account*
```
```
sed -i 's/use_fully_qualified_names = True/use_fully_qualified_names = False/g'
```
```
/etc/sssd/sssd.conf
```
```
systemctl enable sssd
systemctl restart sssd
```

* * *
### Centos 8 Issue
If you have an issue on cento s8 with error message like this,
```
Group Policy Container with DN [cn={967D56A2-2F7C-4109-99DC-FDC4040FE9D4},cn=policies,cn=system,DC=<domain>,DC=local] is unreadable or has unreadable or missing attributes. In order to fix this make sure that this AD object has following attributes readable: nTSecurityDescriptor, cn, gPCFileSysPath, gPCMachineExtensionNames, gPCFunctionalityVersion, flags. Alternatively if you do not have access to the server or can not change permissions on this object, you can use option ad_gpo_ignore_unreadable = True which will skip this GPO. See ad_gpo_ignore_unreadable in 'man sssd-ad' for details.
```
then add the following to the **/etc/sssd/sssd.conf**
```
ad_gpo_ignore_unreadable = True
```


