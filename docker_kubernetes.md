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

## Volumes

- tbd

## Compose

- tbd


[1]: https://www.udemy.com/course/complete-docker-kubernetes
[2]: https://github.com/GoogleContainerTools/distroless
