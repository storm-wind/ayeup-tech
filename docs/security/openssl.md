# Openssl

Some stuff I came across while using openssl

***
### How to get a common name from an SSL certificate

```
openssl x509 -noout -subject -in <path to cert>
```

It will work with .pem etc.


