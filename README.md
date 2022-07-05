# My Docker Cheatsheet

### Containers

To run an image:

```
docker container run <image_name>
```

It downloads the image from docker hub if the image is not available locally.

- `--publish <host_port>:<container_port>`: forwards the host port to container port.

- `--name <container_name>`: assigns an arbitrary name for the container.

To stop a running container:

```
docker container stop <container_name>
```

To start a stopped container:

```
docker container start <container_name>
```

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

To view the processes running inside the container:

```
docker container top <container_name>
```


