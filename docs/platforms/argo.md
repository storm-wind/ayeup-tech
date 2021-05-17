# Argo Tunnels

This is a service from Cloudflare where you can set an outbound port and reach out to the Cloudflare CDN and create secure tunnels while not exposing your server to the web.


### Setup Cloudflared as a Service

You have to make sure that `/root/.cloudflared/config.yml` is valid.

Here is an example,

```
hostname: <servername>

credentials-file: /root/.cloudflared/<uuid for tunnel>.json

tunnel: <tunnel name>
ingress:
 # Rules map traffic from a hostname to a local service:
 - hostname: <yourdomain.com>
   service: https://localhost:<port listening on>

 - hostname: <anotherdoamin.com>
   service: https://<another server IP or port>
```

When you run 

```
cloudflared service install
```
it will pull the config and copy into 

```
/etc/cloudflared
``` 

if its not valid then the systemd install won't work.

Three files are generated in 

```
/etc/systemd/system
```

which are,

```
cloudflared.service
cloudflared-update.service
cloudflared-update.timer
```


### cloudflared.service

```
[Unit]
Description=Argo Tunnel
After=network.target

[Service]
```

