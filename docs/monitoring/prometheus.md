# All things Prometheus

---

### How to install node-exporter

A node-exporter is effectively an agent which talks to Prometheus server to send metrics to it from a host.

Here is how to set it up.

Download latest node-exporter
```
wget -O node-exporter.tar.gz https://github.com/prometheus/node_exporter/releases/download/v1.0.1/node_exporter-1.0.1.linux-armv7.tar.gz
tar -xvf node-exporter.tar.gz --strip-components=1
```

I usually put this is **/opt**

Now setup as service, make sure you have a user existing for this service.

Setup user without login
```
useradd -s /sbin/nologin prometheus
```

Create systemctl service
```
nano /etc/systemd/system/nodeexporter.service

[Unit]
Description=Prometheus Node Exporter
Documentation=https://prometheus.io/docs/guides/node-exporter/
After=network-online.target

[Service]
User=prometheus
Restart=on-failure

ExecStart=/opt/node-exporter/node_exporter

[Install]
WantedBy=multi-user.target
```
```
systemctl daemon-reload
```

---




