# My Docker Cheatsheet

### Managing Containers

To run an image:

```
docker container run <image_name> [command]
```

It downloads the image from docker hub if the image is not available locally.

- `--publish <host_port>:<container_port>`: forwards the host port to container port.

- `--name <container_name>`: assigns an arbitrary name for the container.

- `-e ENV_NAME=VALUE`: sets an environment variable inside the container.

- `-it`: starts a new interactive container

To stop a running container:

```
docker container stop <container_name>
```

To start a stopped container:

```
docker container start <container_name>
```

To run a new command on an existing container:

```
docker container exec <container_name> <command>
```

- `-it`: runs the command interactively.

To view all running containers:

```
docker container ls
```

- `-a`: lists all containers including the stopped containers

To view the logs of a container:

```
docker container logs <container_name>
```

To remove a container:

```
docker container rm <container_name>
```

Notice that the container must be stopped before removing.

- `-f`: forces the container to be stopped and removed.

To view exposed ports inside a container:

```
docker container port <container_name>
```

To view the processes running inside the container:

```
docker container top <container_name>
```

To view live performance data on all containers:

```
docker container stats 
```

To show metadata about the container (startup, volumes, networking, etc):

```
docker container inspect <container_name>
```

- `--format`: formats the output of docker inspect. e.g: `--format '{{ .NetworkSettings.IPAddress }}'` to get the IP address of the container.

### Networking

To create a network:

```
docker network create <network_name>
```

- `--driver`: to set the driver of the network (bridge, host, null)

To see a list of networks:

```
docker network ls
```

To see metadata about a network:

```
docker network inspect <network_name>
```

To dynamically attach a network to a container:

```
docker network connect <network_name> <container_name>
```

To dynamically disconnect a network from a container:

```
docker network disconnect <network_name> <container_name>
```
