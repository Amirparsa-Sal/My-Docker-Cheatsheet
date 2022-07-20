# My Docker Cheatsheet

## Containers Commands

To run an image:

```
docker container run <image_name> [command]
```

It downloads the image from docker hub if the image is not available locally.

- `--publish <host_port>:<container_port>`: forwards the host port to container port.

- `--name <container_name>`: assigns an arbitrary name for the container.

- `-e <env_name>=<env_value>`: sets an environment variable inside the container.

- `-it`: starts a new interactive container

- `--net`: specifies the network of the container

- `--net-alias`: adds an alias DNS record  for the container

- `--rm`: to remove the container automatically after finishing its job.

- `-v <vol_name>:<vol_path>`: specifies a volume for the container

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

## Networking Commands

To create a network:

```
docker network create <network_name>
```

- `--driver`: to set the driver of the network (bridge, host, null)

To remove a network:

```
docker network rm <network_name>
```

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

Note: Containers on the same custom network can access each other using their names instead of their IP address. For the bridge network we must use `-link` to link containers.

## Images Commands

To pull an image from the registry:

```
docker image pull <image_name>
```

To see a list of pulled images:

```
docker image ls
```

To remove an image:

```
docker image rm <image_name>
```

To see the image's changes history:

```
docker image history <image_name>
```

To see metadata about the image:

```
docker image inspect <image_name> 
```

To assign a new tag to an image:

```
docker image tag <src_tag> <dst_tag>
```

To push an image:

```
docker image push <image_name>
```

To build an image using a Dockerfile:

```
docker image build <dockerfile_path>
```

- `-t`: Tags the image after building

- `-f`: specifies the name of the Dockerfile

## Volume Commands

To create new volume:

```
docker volume create <volume_name>
```

To view all volumes:

```
docker volume ls
```

To remove a volume:

```
docker volume rm <volume_name>
```

To remove all unused volumes:

```
docker volume prune
```

To see metdata about a volume:

```
docker volume inspect <volume_name>
```

## Dockerfile

#### FROM

Specifies the base image of the image:

```dockerfile
FROM <image_name>
```

#### RUN

Runs a command:

```dockerfile
RUN <command1> && <command2> ...
RUN ["executable", "param1", "param2"]
```

- Putting `&&` between the commands ensures that they will create only one layer in image.

#### ENV

Sets an environment variable:

```dockerfile
ENV <variable_name>=<variable_value> ...
ENV <variable_name> <<variable_value>
```

- We can use env variables in dockerfile like this: `${variable_name}`

#### WORKDIR

Changes the current working directory inside the container:

```dockerfile
WORKDIR <path>
```

#### COPY

Copies files or directories from our system into the container:

```dockerfile
COPY <src_path> <dest_path>
```

#### CMD

Provides defaults for an executing container:

```dockerfile
CMD ["executable", "param1", "param2"]
CMD ["param1", "param2"]
CMD <command>
```

- There can only be one `CMD` instruction in a `Dockerfile`. If you list more than one `CMD` then only the last `CMD` will take effect.

#### EXPOSE

 Informs Docker that the container listens on the specified network ports at runtime:

```dockerfile
EXPOSE <port> [<port>/protocol]
```

- The `EXPOSE` instruction does not actually publish the port. It functions as a type of documentation between the person who builds the image and the person who runs the container

#### VOLUME

Creates a mount point with the specified name:

```dockerfile
VOLUME ["vol_path"]
VOLUME <vol_path> ...
```

## Docker-Compose

```yaml
services:
  service1:
    build: 
      context: .
      dockerfile: <dockerfile_name>
    image: <image1_name>
    ports:
      - '<host_port1>:<container_port1>'
      - '<host_port2>:<container_port2>'
    volumes:
      - <bind_path1> or <named_vol1>:<container_path1>
      - <bind_path2> or <named_vol2><container_path2>
    environment:
      - <key1>=<value1>
      - <key2>=<value2>
    depends_on:
      - "service2"

  service2:
    ...

volumes:
  <named_vol1>:
  <named_vol2>:  

network:
    ...
```

To run the docker-compose file;

```
docker-compose up
```

- `-f`: specifies the name of the docker-compose file

- `-d`: runs docker-compose in background

   

To stop the running docker-compose:

```
docker-compose down
```

- `-v`: removes all volumes defined in volumes section after stopping

- `--rmi type` : removes images after downing the docker-compose
  
  - `type=all`: removes all images
  
  - `type=local`: removes only images that don't have custom tag set by the `image` field.

To rebuild images that are created using docker-compose:

```
docker-compose build
```
