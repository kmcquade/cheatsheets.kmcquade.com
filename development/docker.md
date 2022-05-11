# Docker

## Cleanup

### Remove containers matching a name

```
export MATCH_IMAGE=bkimminich/juice-shop
docker ps -a | awk '{ print $1,$2 }' | grep $MATCH_IMAGE | awk '{print $1 }' | xargs -I {} docker rm {}
```

### List all containers (only IDs)

```
docker ps -aq
```

### Stop all running containers

```
docker stop $(docker ps -aq)
```

### Remove all containers

```
docker rm $(docker ps -aq)
```

### Remove all images

```
docker rmi $(docker images -q)
```

### One-liner Docker Cleanup for Memory Management)&#x20;

```
#!/usr/bin/env bash
set -x

docker stop $(docker ps -a -q)
yes | docker system prune
yes | docker system prune --volumes
docker rm $(docker ps -a -q)
docker images -q --filter "dangling=true" | xargs docker rmi
docker rmi $(docker images -q)
```

## Networking

### Get IP Address of Docker image

```
docker inspect --format '{{ .NetworkSettings.Networks.IPAddress }}' imagename
```

## Startup

### Override entrypoint

```
docker run -it  --entrypoint "/bin/bash" --rm owner/image:latest
```

## Troubleshooting

### View container logs

Docker logs command, as shown [here](https://docs.docker.com/engine/reference/commandline/logs/):

```
docker logs --details containername
```

