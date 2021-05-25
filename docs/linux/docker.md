# Docker

Some stuff I came across while playing with docker.

***
### Stop a difficult container

Sometimes some containers just won't stop or die when you tell them to, so you use this command and it seems to work.
```
docker kill ----signal=SIGINT <container id>
```

more info here https://www.ctl.io/developers/blog/post/gracefully-stopping-docker-containers/


***
## Docker Commands
***
### Portainer Docker cmd

```
docker run -d -p 9000:9000  --restart always --name portainer -v /<volume location>/portainer/data:/data -v /var/run/docker.sock:/var/run/docker.sock portainer/portainer-ce

```

### Handling Docker images

Save a docker image ready to be deployed to another server.

```
docker save -o <image-name>.tar <image-name>
```

A breakdown of the command

```
docker save -o <image-name> <commited image name>
```

### Deploy Docker image to server

```
docker load -i <image-name.tar
```

***


# Docker Scripts

***

### Docker cleanup script

This script deletes docker containers that haven't being running for a few days.

```
#!/bin/bash
# Script to clean up docker containers no longer running after a few days

exec 2>&1
{
        /usr/bin/docker rm $(docker ps -a | grep -v Up\ | grep  '.*Exited\ .*days\ .*' | awk '{print $1}')

}
```


# docker-compose

Deploy multiple contianers in a stack using docker-compose, my preferred way of using docker.

### Firefly docker-compose example

```
---
networks:
  firefly_iii_net:
    driver: bridge
services:
  firefly_iii_app:
    image: jc5x/firefly-iii:latest
    depends_on:
      - firefly_iii_db
    networks:
      - firefly_iii_net
    ports:
      - "8081:80"
    env_file: .env
    volumes:
      -
        source: firefly_iii_export
        target: /var/www/firefly-iii/storage/export
        type: volume
      -
        source: firefly_iii_upload
        target: /var/www/firefly-iii/storage/upload
        type: volume
  firefly_iii_db: 
    image: "postgres:10"
    environment:
      - POSTGRES_PASSWORD=secret
      - POSTGRES_USER=firefly
    networks: 
      - firefly_iii_net
    volumes: 
      - firefly_iii_db:/var/lib/postgresql/data
version: "3.2"
volumes: 
  firefly_iii_db: ~
  firefly_iii_export: ~
  firefly_iii_upload: ~
```
