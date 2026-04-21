# Docker and Kubernetes: Zero to Hero

Notes on the [course][1] from Udemy.

## General

- Run a container in detached mode to avoid cluttering shell output via `docker run --detach`
- To get its logs or inspect it, run `docker ps --all` to get its ID, and either run `docker logs --follow <CONTAINER_ID>` or `docker inspect`
- You can pause containers using `docker pause <CONTAINER_ID>`.

## Architecture

- Docker client is a CLI which  makes API requests to the Docker host
- Docker host exposes API, runs daemon, caches images and thus is able to run containers

## Dockerfiles

tbd

## Compose

-


[1]: https://www.udemy.com/course/complete-docker-kubernetes
