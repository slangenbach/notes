# CKAD: Certified Kubernetes Application Developer

Notes on the [course][1] from Udemy.

## General

- [nerdctl][2] is the CLI for [containerd][3]
- [crictl][4] is the CLI to debug container runtimes

## CLI

- Use `kubectl get all` to get all resources in the cluster
- Use `kubectl run <NAME> --image=<IMAGE_NAME> --dry-run=client -o yaml > manifest.yml` to easily create a manifest file
- Use `kubectl edit deployment <DEPLOYMENT_NAME>` to edit a deployment in place
- Use `kubectl scale deployment <NAME> --replicas=<NUM_REPLICAS>` to scale a deployment
- Use `kubectl replace <RESOURCE> <RESOURCE_NAME>` to replace resources
- Consider using `-o wide` to display additional details

## Exam advice

- Read through the [notes][5] of previous students

[1]: https://www.udemy.com/course/certified-kubernetes-application-developer
[2]: https://github.com/containerd/nerdctl
[3]: https://containerd.io/
[4]: https://kubernetes.io/docs/tasks/debug/debug-cluster/crictl/
[5]: https://github.com/lucassha/CKAD-resources
