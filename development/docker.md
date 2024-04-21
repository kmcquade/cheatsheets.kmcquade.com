# Docker

## Quick reference

I'm refreshing this cheat sheet with commands I use. Will update as I go along - or I won't. Don't expect anything.

* Script to run a container with an interactive shell

Run as default user: `docker-shell.sh debian debian:stable-slim`.  This will give you a shell on a container with that image as the default user.

Run as root user: `docker-shell.sh debian debian:stable-slim --root` . This will give you a shell on a container with that image as the root user.

```
#!/usr/bin/env bash

# Usage:
# Run as default user:
#   docker-shell.sh debian debian:stable-slim
# Run as root user
#   docker-shell.sh debian debian:stable-slim --root

run() {
  if [ "$#" -lt 2 ]; then
    echo "Usage: docker-shell.sh <name> <image> [--root]"
    exit 1
  fi

  NAME="$1"
  IMAGE="$2"
  USER_ARG=""

  # Check if the last argument is --root
  if [[ "$3" == "--root" ]]; then
    USER_ARG="-u root"
  fi

  export DOCKER_DEFAULT_PLATFORM=linux/amd64
  docker run -it --rm --name $NAME $USER_ARG $IMAGE /bin/bash
}

# Handle the input arguments. This assumes the script is called with at least two arguments.
if [[ "${@: -1}" == "--root" ]]; then
  # If the last argument is --root, pass the first two arguments and the root flag to run
  run "$1" "$2" "--root"
else
  # Otherwise, pass all arguments as they are
  run "$@"
fi
```

## # Old

### Cleanup

#### Remove containers matching a name

```
export MATCH_IMAGE=bkimminich/juice-shop
docker ps -a | awk '{ print $1,$2 }' | grep $MATCH_IMAGE | awk '{print $1 }' | xargs -I {} docker rm {}
```

#### List all containers (only IDs)

```
docker ps -aq
```

#### Stop all running containers

```
docker stop $(docker ps -aq)
```

#### Remove all containers

```
docker rm $(docker ps -aq)
```

#### Remove all images

```
docker rmi $(docker images -q)
```

#### One-liner Docker Cleanup for Memory Management)&#x20;

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

### Networking

#### Get IP Address of Docker image

```
docker inspect --format '{{ .NetworkSettings.Networks.IPAddress }}' imagename
```

### Startup

#### Override entrypoint

```
docker run -it  --entrypoint "/bin/bash" --rm owner/image:latest
```

### Troubleshooting

#### View container logs

Docker logs command, as shown [here](https://docs.docker.com/engine/reference/commandline/logs/):

```
docker logs --details containername
```

