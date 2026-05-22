# Docker and Kubernetes: Zero to Hero

Notes on the [course][1] from Udemy.

## Docker

### General

- Run containers in detached mode to avoid cluttering shell output via `docker run --detach`
- To get container logs or inspect it, run `docker ps --all` to get its ID, and either run `docker logs --follow <CONTAINER_ID>` or `docker inspect`
- You can pause containers using `docker pause <CONTAINER_ID>`.

### Architecture

- Docker client is a CLI making API requests to the Docker host
- Docker host exposes API, runs daemon, caches images and thus is able to run containers

### Dockerfiles

- Inspect the layers of an image with `docker history <IMAGE_NAME>`
- Use `ENTRYPOINT` to specify the main executable for a container, and when you want to enable users to pass additional parameters to the container on startup. If using `CMD` instead, parameter passed to the container at run time will override the default command
- Use multi-stage builds with alpine and [distroless][2], *non-root* images for production workloads
- When using compiled languages, consider using dedicated build, deps and run stages to minimize image size

### CLI

- We can set environment variables from env files via the CLI using `--env-file ".env"`
- We create volumes using the CLI via `docker volume create`

### Volumes

- *Bind mounts* link files from the host system to the container. You may use them for local developments for frameworks supporting hot-reloading, but generally [Compose Watch][3] is preferred
- *Named volumes* (a.k.a. simply volumes) persist data and can be reused across containers

### Resource Management

- Consider setting hard limits on cpu and memory usage to prevent containers impacting other containers and/or the docker host

### Restarts

- Containers can be automatically restarted via the `--restart always` flag
- Containers can be restarted on failure via the `--restart on-failure:<RESTART_COUNT>` flag

### Networking

- Options for networking include *bridge* (default, private network on host machine), *host* (no isolation), *overlay* (for distributed systems) and *none*
- Note that containers can only be reached via hostnames when using user-defined networks

### Compose

- We can read environment variables from env files via the `env_file` option
- Use the [watch][3] option in combination with [profiles][4] for local development
- Use [includes][5] to modularize compose files

## K8s

### General

- Use [k9s][6] to interact with k8s
- Use `kubectl api-resources` to get an overview of all k8s resources

### Architecture

- K8s distinguishes between control (brain) and data (muscle) planes
- The control plane contains master nodes, data planes contains worker nodes
- Master nodes contain API server (main entry point for all admin tasks), etcd (key-value store for cluster configuration and state data), scheduler (to place pods on nodes), controller manager and cloud controller manager (to interact with cloud infrastructure)
- Worker nodes contain kubelet (agent, ensures that containers are running in pods as specified), container runtime (containerd, crio-o, etc.) kube-proxy (networking )nd other k8s objects (services, deployments, jobs, replica sets, stateful sets, etc.)

### Configuration

- Manifest files are written in YAML
- They contain the *apiVersion*, *kind*, *metadata* and *spec* top-level fields
- Use `kubectl apply` and `kubectl delete` to modify k8s objects
- We can store configuration for multiple objects within a single YAML file by adding `---` between them. Usually it's better to use a dedicated configuration file per object

### Pods

- Pods are a single instance of a running process in the cluster
- They are a higher-level abstraction than working with containers directly
- Can contain one or more containers
- Containers within a pod can communicate with each other, share storage and network and write to the same values
- Per *default* all pods in k8s can communicate with each other

### Services

- Services provide stable IP addresses and DNS for pods
- Pods are assigned to services using selectors which apply to labels
- Use *ClusterIP* for exposing a a service internally
- Use *NodePort* for exposing a service externally without load balancing
- Use *LoadBalancer* for exposing a service externally *with* a third-party load balancer
- Use *ExternalName* to allow services within the cluster to access external services via DNS name

### Replica Sets

- Replica sets make sure pods keep running as specified, e.g. in case of failures
- They use a template and the number of replicas to create pods
- We usually interact with ReplicaSets via Deployments, as modifying replica sets won't modify existing pods automatically

### Deployments

- Deployments are a higher level abstraction of pods and replica sets
- Deployments provide rolling updates, rollbacks and allow us to specify replicas
- We can manage rollouts via `kubectl rollout`
- Use *kubernetes.io/change-cause* annotation to populate the *CHANGE-CAUSE* in rollout history
- If deployments are failing, describe the pod and investigate events

### Resource Management

#### Namespaces

- Use namespaces to logically isolate groups of resources, enforce RBAC and set resource quotas
- Default namespaces include: default, kube-system, kube-public and kube-node-release
- Set the default namespace via `kubectl config set-context --current --namespace=<NAMESPACE_NAME>`
- To make pods communicate across namespaces, use the FQDN, e.g. <POD_NAME.NAMESPACE.CLUSTER_NAME>

#### Labels

- Labels are key-value pairs which contain identifying metadata
- They are *not* unique
- Use labels to group k8s resources, then use selectors to match labels
- We can use *equality*-based (matchLabels), and *set*-based (matchExpression) selectors
- Consider using the *managed* key with *matchExpressions* (key: managed, operator: Exists) to easily select resources

#### Annotations

- Annotations contain *non*-identifying metadata
- Annotations contain a prefix (DNS subdomain) and a name
- Use annotations to provide configuration to resources

#### Quotas, Requests and Limits

- Quotas are applied at the *namespace* level and define hard limits on resource usage
- Requests and Limits are applied at the *pod-level*
- Requests specify the minimum resources a container needs, while limits specify the maximum resources a container can consume
- Make sure to quotas contain enough resources to handle rollouts

#### Probes

- Probes are periodic health checks performed by k8s
- We can use *startup*, *readiness* and *liveness* probes
- Readiness probes check if the container is ready to accept traffic (keeps executing throughout the container lifecycle)
- Liveness probes check if the container is still running (keeps executing throughout the container lifecycle)

### Storage

- Volumes are directories which are accessible to containers in a pod
- Volume types include *emptyDir* (ephemeral), *local* (persistent, defined at node level, requires configuring persistent volume node affinity), *persistent volume (PV)* (requires persistent volume claims, PVC, for usage in pods), *ConfigMaps* and *Secrets*
- PVs have 1-to-1 relationship to Persistent Volume Claims
- PV access types include *ReadWriteOnce* (can be mounted by *single* node), *ReadWriteOncePod* (can be mounted by a single *pod*), *ReadWriteMany*, *ReadOnlyMany*
- Custom storage solution are supported by the [Container Storage Interface (CSI)][7]

#### Stateful Sets

- Stateful Sets (SS) support us in managing stateful applications
- They provide stable and unique network identities, provide consistent storage and enable us to create, scale and delete pods in order
- Note that SS create one replica per PVC per replica
- Use *headless* services (by specifying *clusterIp: None* in a service definition) to provide a stable DNS entry to talk to specific pods

### Configuration

- Use config maps to provide configuration to pods via environment variables or as files via volume mounts
- When making ConfigMaps available as files, each key-value pair in the map will be exposed as a dedicated file
- A config map must exist in the same namespace as the pod using it
- Consider keeping dedicated config maps for environment variables and files
- Consider reading env values via `valueFrom configMapKeyRef` if environment variable names different across pods

### Secrets

- Per default secrets are **not** encrypted by default, but just base64 encoded
- Secrets can be provided as environment variables via `secretRef` or as files via volume mounts

### Security

#### RBAC

- Core security constructs in k8s include RBAC, network policies, encryption (at rest), pod security standards (PSS)
- RBAC can be enforced on users, groups and service accounts
- Roles focus on resources within a specific namespace, while cluster roles are enforced on the entire cluster
- Role bindings assign roles to users, groups and service accounts; Cluster role bindings do the same for cluster roles
- Use k8s API groups, resources and subresources to implement fine-grained permissions
- Service accounts (SA) are intended for applications, so that they can interact with the k8s API
- SAs are namespaced

#### Network Policies

- By default, all traffic between pods is allowed
- A network policy contains assignments to pods, ingress and egress rules
- Be aware that network policies are namespace-scoped
- In order for network policies to have an effect, we ne need a [CNI][8] plugin such as [Calico][9] or [Cilium][10]
- Depending on how network policies are constructed, rules either add up (logical AND) or are applied selectively (logical OR)
- Start with a deny-all policy and only allow communication where necessary
- In order to allow pod A to communicate to pod A, you should create a policy that allows traffic to the k8s DNS in order to use services correctly

#### Pod Security Standards

- PSS apply to pods and define security contexts
- Contexts are enforced by the security admission controller and can be applied on the namespace level using labels
- The Controller can set the modes *enforce, warn or audit* and set the levels *restricted, baseline or privileged*

### Kustomize

- Kustomize is built into `kubectl` and allows us to manage configuration of similar applications across environments
- Bases, overlays and patches are key concepts: A *base* is a set of manifests shared across an environment, while an *overlay* applies environment-specific customizations on top of a base
- Kustomize supports customization of [various objects][11]
- Kustomize can dynamically generate config maps (from standard and env files) and secrets from files and environment variables
- Patches allow us to write partial specification which are then merged with existing objects
- Merge patches are useful for customizing configuration for specific resources
- JSON patches are useful to remove configuration for specific resource
- Make sure to create a dedicated YAML file per patch


[1]: https://www.udemy.com/course/complete-docker-kubernetes
[2]: https://github.com/GoogleContainerTools/distroless
[3]: https://docs.docker.com/compose/how-tos/file-watch/
[4]: https://docs.docker.com/compose/how-tos/profiles/
[5]: https://docs.docker.com/reference/compose-file/include/
[6]: https://k9scli.io/
[7]: https://kubernetes-csi.github.io/docs/
[8]: https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/
[9]: https://docs.tigera.io/calico/latest/getting-started/kubernetes/quickstart
[10]: https://docs.cilium.io/en/stable/overview/intro/
[11]: https://kubectl.docs.kubernetes.io/references/kustomize/kustomization/
