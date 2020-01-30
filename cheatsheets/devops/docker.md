# Docker

### Get IP Address of Docker image

```text
docker inspect --format '{{ .NetworkSettings.Networks.IPAddress }}' imagename
```

### Override entrypoint

```text
docker run -it  --entrypoint "/bin/bash" --rm owner/image:latest
```

### Docker Cleanup \(Memory Management\) 

```text
#!/usr/bin/env bash
set -x

docker stop $(docker ps -a -q)
yes | docker system prune
yes | docker system prune --volumes
docker rm $(docker ps -a -q)
docker images -q --filter "dangling=true" | xargs docker rmi
docker rmi $(docker images -q)
```

### Docker Troubleshooting

Docker logs command, as shown [here](https://docs.docker.com/engine/reference/commandline/logs/):

```text
docker logs --details containername
```



