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


