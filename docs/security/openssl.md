# Openssl

Some stuff I came across while using openssl

***
### How to get a common name from an SSL certificate

```
openssl x509 -noout -subject -in <path to cert>
```

It will work with .pem etc.


---

### Remove a host from an authorized Key
```
ssh-keygen -R hostname
```

---
### Basic SSH Hardening

Only allow root logins via local terminal

```
echo "tty1" > /etc/securetty
chmod 700 /root
```

---

