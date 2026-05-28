# CKAD: Certified Kubernetes Application Developer

Notes on the [course][1] from Udemy.

## General

- [nerdctl][2] is the CLI for [containerd][3]
- [crictl] is the CLI to debug container runtimes

## Architecture

- K8s supports different container runtimes via the container runtime interface (CRI)
- Containers itself are supported via the Open Container Initiative

## CLI

- Use `kubectl get all` to get all resources in the cluster
- Use `kubectl run <NAME> --image=<IMAGE_NAME> --dry-run=client -o yaml > manifest.yml` to easily create a manifest file
- Use `kubectl edit pod <POD_NAME>` to edit a pod in place
- Use `kubectl scale deployment <NAME> --replicas=<NUM_REPLICAS>` to scale a deployment
- Use `kubectl replace <RESOURCE> <RESOURCE_NAME>` to replace resources
- Consider using `-o wide` to display additional details


[1]: https://www.udemy.com/course/certified-kubernetes-application-developer
[2]: https://github.com/containerd/nerdctl
[3]: https://kubernetes.io/docs/tasks/debug/debug-cluster/crictl/
