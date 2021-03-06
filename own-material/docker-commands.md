# Some commands

Show info about docker installation

```
docker version
```

```
docker info
```

Run a nginx container locally at port `8080`

```
docker container run --publish 8080:80 nginx
```

Run a nginx container locally at port `8080` and detach

```
docker container run --publish 8080:80 --detach nginx
```

List running containers

```
docker container ls
```

Stop a container which DI uniquely starts with `ID-prefix`

```
docker container stop <ID-prefix>
```

List all containers, not just the ones that are currently running

```
docker container ls --all
```

Use `--name <name>` to explicitly define a name for your container

```
docker container run --publish 8080:80 --name <name> nginx
```

To see the logs of a container

```
docker container logs <name>
```

To see the logs of a container and keep looking for new entries

```
docker container logs -f <name>
```

To see the processes inside a container with name `<name>`

```
docker container top <name>
```

To remove containers `n` container

```
docker container rm <name_1> <name_2> ... <name_n>
```

List images found locally

```
docker image ls
```

See info about a container

```
docker container inspect <name>
```

```
docker container stats
```

To execute a process in a running container, e.g. bash, interactively, run

```
docker container exec --tty --interactive <name> bash
```

See port configuration for container `<name>`

```
docker container port <name>
```

Find out the IP that my container has

```
docker container inspect --format '{{ .NetworkSettings.IPAddress }}' <name>
```

List all networks

```
docker network ls
```

See the current state and properties of a network called `<name>`

```
docker network inspect <name>
```

Creates a new network with the default driver `bridge`

```
docker network create <name>
```

Create a container attached to a specific network

```
docker container run -p 81:80 -d --network <network_name> --name <container_name> nginx
```

```
docker network create --driver
```

Add a container to a network (a container may be in several networks at a time)

```
docker network connect <network_name> <container_name>
```

Remove a container from a network

```
docker network disconnect <network_name> <container_name>
```

Check that two containers can talk to each other through docker dns service

```
docker container exec --interactive --tty <container_name1> ping <container_name2>
```

Create a couple of `elasticsearch` containers, both using the dns alias `elastic`

```
docker container run -d --net sample_net3 --network-alias elastic --name elastic1 elasticsearch:6.4.0
docker container run -d --net sample_net3 --network-alias elastic --name elastic2 elasticsearch:6.4.0
```

Create container of image `alpine`, added to the network `sample_net3`, and execute `nslookup` to `elastic`. `--rm` is used to remove the container immediately after the command is run

```
docker container run --net sample_net3 --rm alpine nslookup elastic
```

Run `centos` container, added to network `sample_net3`, and executes `curl` to domain name `elastic` on port 9200, where elastic containers are listening

```
docker container run --rm --net sample_net3 centos curl -s elastic:9200
```

To see the layers of an image

```
docker image history <name>
```

To inspect the configuration and how to use an image

```
docker image inspect <name>
```

To create a new image tag from an existing one

```
docker image tag <name>:<tag> <new-name>:<new-tag>
```

To push an image

```
docker image push <name>:<tag>
```

To login/logout

```
docker login
docker logout
```

To create a new image from a dockerfile

```
docker image build --tag <image_name>:<version/tag> <path_to_dockerfile>
```

To clean all docker objects

```
docker system prune
```

To clean containers

```
docker container prune
```

To clean images

```
docker image prune
```

To manually create a volume

```
docker volume create <volume_name>
```

To specify a container which volume to use:

```
docker container run --volume <name>:<path> <image_name>
```

e.g. mysql image uses a volume in /var/lib/mysql (check https://github.com/docker-library/mysql/blob/69e4645ced5c687b861ce4a4e8aa3ca4600152b6/8.0/Dockerfile#L69), so it would be:

```
docker container run --volume mysql_volume:/var/lib/mysql mysql:latest
```

To create a bind mount, use a path instead of a name when running a container with a specific volume:

```
docker container run --volume /some/path/in/the/host:/path/in/the/container <image_name>
```

To list images filtered byt by repositories, e.g. `postgres`:

```
docker image ls --filter=reference=postgres
```

Docker compose

```
docker-compose up
docker-compose up -d
docker-compose down
docker-compose logs
docker-compose logs -f
```

Enable swarm in your docker server

```
docker swarm init
```

List all the service you have

```
docker service ls
```

Create a service

```
docker service create --name <service_name> <image_name> <command> <command-args>
```

e.g.

```
docker service create alpine ping 8.8.8.8
```

See all the tasks (containers) in your service

```
docker service ps <service_name>
```

Update the number of tasks/containers that a service has

```
docker service update --replicas <N> <service_name>
```

## docker swarm

To initialize a node as a swarm leader

```
docker swarm init --advertise-addr <ip_address>
```

To list all nodes in a swarm, run

```
docker node ls
```

After that you'll get a token that can be sued by other nodes to join that swarm, something like

```
docker swarm join --token SWMTKN-1-15d49s35mx7qyxjed6ixcu2ovrw8neq3uhgc7wwk6jx3b02gpq-2a7h7dqi6j0yr2s3j5d3vnqem 192.168.0.46:2377
```

To obtain a manager token, run

```
docker swarm join-token manager
```

To update the role of a node

```
docker node update --role <role> <node>
```

Some services commands

```
docker service create -p <local_port>:<container_port> --name <service_name> <image>
```

```
docker service scale <service_name>=<instances_number>
```

```
docker service update --image myapp:1.0.1 <service_name>
```

```
docker stack deploy <stack_name>
```

Healthchecks:

```
docker container run --health-cmd="<image_health_command> || exit 1" <image>
```

```
docker service create --health-cmd="<image_health_command> || exit 1" <image> <service_name>
```

To create your own local registry:

```
docker container run --name registry -p 5000:5000 -d -v $(pwd)/registry-data:/var/lib/registry registry
```

Now, you can visit `http://localhost:5000/v2/_catalog`
