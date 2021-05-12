# Podman

Podman is Redhats answer to Docker. You can un docker continaers as a **systemd** service.


### Bind podman container to local directory

sudo podman run -it -p 80:80 --name nginx-proxy -v /opt/podman/nginx/conf.d:/etc/nginx/conf.d nginx sh
