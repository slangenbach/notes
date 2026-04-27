# Docker and Kubernetes: Zero to Hero

Notes on the [course][1] from Udemy.

## General

- Run containers in detached mode to avoid cluttering shell output via `docker run --detach`
- To get container logs or inspect it, run `docker ps --all` to get its ID, and either run `docker logs --follow <CONTAINER_ID>` or `docker inspect`
- You can pause containers using `docker pause <CONTAINER_ID>`.

## Architecture

- Docker client is a CLI making API requests to the Docker host
- Docker host exposes API, runs daemon, caches images and thus is able to run containers

## Dockerfiles

- Inspect the layers of an image with `docker history <IMAGE_NAME>`
- Use `ENTRYPOINT` to specify the main executable for a container, and when you want to enable users to pass additional parameters to the container on startup. If using `CMD` instead, parameter passed to the container at run time will override the default command
- Use multi-stage builds with alpine and [distroless][2], *non-root* images for production workloads
- When using compiled languages, consider using dedicated build, deps and run stages to minimize image size

## CLI

- We can set environment variables from env files via the CLI using `--env-file ".env"`
- We create volumes using the CLI via `docker volume create`

## Volumes

- *Bind mounts* link files from the host system to the container. You may use them for local developments for frameworks supporting hot-reloading, but generally [Compose Watch][3] is preferred
- *Named volumes* (a.k.a. simply volumes) persist data and can be reused across containers

## Resource Management

- Consider setting hard limits on cpu and memory usage to prevent containers impacting other containers and/or the docker host

## Restarts

- Containers can be automatically restarted via the `--restart always` flag
- Containers can be restarted on failure via the `--restart on-failure:<RESTART_COUNT>` flag

## Networking

- Options for networking include *bridge* (default, private network on host machine), *host* (no isolation), *overlay* (for distributed systems) and *none*
- Note that containers can only be reached via hostnames when using user-defined networks

## Compose

- We can read environment variables from env files via the `env_file` option
- Use the [watch][3] option in combination with [profiles][4] for local development
- Use [includes][5] to modularize compose files


[1]: https://www.udemy.com/course/complete-docker-kubernetes
[2]: https://github.com/GoogleContainerTools/distroless
[3]: https://docs.docker.com/compose/how-tos/file-watch/
[4]: https://docs.docker.com/compose/how-tos/profiles/
[5]: https://docs.docker.com/reference/compose-file/include/
