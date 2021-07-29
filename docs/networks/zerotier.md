# How to install Zerotier

**Basic install**

```
curl -s https://install.zerotier.com | sudo bash
```


**More secure**

```
curl -s 'https://raw.githubusercontent.com/zerotier/ZeroTierOne/master/doc/contact%40zerotier.com.gpg' | gpg --import && \

if z=$(curl -s 'https://install.zerotier.com/' | gpg); then echo "$z" | sudo bash; fi
```
To Join a network

From the command line simply type 

```
zerotier-cli join ################ with ############### being the 16-digit network ID of the network you wish to join.
```
