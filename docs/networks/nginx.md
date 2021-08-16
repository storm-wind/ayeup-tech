# Nginx conf example for Bitwarden


![](img/nginx-logo.png)



This example points at a docker container running on the same host.


```
server  {
  server_name example.domain.com;
  location  / {
    proxy_redirect off;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Forwarded-Proto $scheme;
    proxy_set_header X-Forwarded-Protocol $scheme;
    proxy_set_header X-Url-Scheme $scheme;
    proxy_pass  http://127.0.0.1:8080/;

    client_max_body_size 0;

    }

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/example.domain.com/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/example.domain.com/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
}

server  {
    if ($host = example.domain.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


  server_name example.domain.com;
    listen 80;
    return 404; # managed by Certbot
```
