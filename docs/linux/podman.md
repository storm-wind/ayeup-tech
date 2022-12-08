# Podman

Podman is Redhats answer to Docker. You can un docker containers as a **systemd** service.


### Bind podman container to local directory

```
sudo podman run -it -p 80:80 --name nginx-proxy -v /opt/podman/nginx/conf.d:/etc/nginx/conf.d nginx sh
```

### Install Podman on RHEL 9

Run the following to install

```
dnf install git
dnf install python3-pip
pip3 install --upgrade pip
pip3 install podman-compose
```

#### Some basic commands for podman-compose 
To get list of commands just run **podman-compose**

Most commands are like using **docker-compose** when you have a docker-compose file. So you can create a docker-compose file and then run,
```
podman-compose pull
podman-compose up
```
or
```
podman-compose up -d
```



