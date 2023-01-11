# kubernetes hands on

A selection of hands on exercises for Kubernetes (K8s) training.

The exercises introduce Kubernetes concepts in a specific order.

A summary of most of the commands used in the exercises is available at the
[cheatsheet.md](cheatsheet.md).


## The basics

- [01-pods-deployments.md](01-pods-deployments.md)
- [02-declarative-deployment.md](02-declarative-deployment.md)
- [03-service-discovery.md](03-service-discovery.md)
- [04-rolling-updates.md](04-rolling-updates.md)
- [05-storage.md](05-storage.md)


## Setup

You can set up a local cluster with [Docker
Desktop][docker-desktop] or [Kind][kind] or [Minikube][minikube].

Once you have access to a cluster, the following exercises will help you understand the main Kubernetes concepts.

- [setup-namespace](setup/namespace.md) - Skip if you've
  already created a personal namespace and set it as your default.

### kubectl autocompletion

On Linux, using bash, run the following commands:

```shell
echo "source <(kubectl completion bash)" >> ~/.bashrc
. ~/.bashrc
```

The commands above will enable kubectl autocompletion when you start a new bash
session and source (reload) bashrc i.e. enable kubectl autocompletion in your
current session.

See: [Kubernetes.io - Enabling shell autocompletion][autocompletion] for more
info.

# Cheatsheet

A collection of useful commands to use throughout the exercises:

```
kubectl api-resources         # List resource types


kubectl explain <resource>    # Show information about a resource
kubectl explain deployment


# List resources in cluster
kubectl get <resource>                    # In current namespace
kubectl get <resource> -n <namespace>     # In specific namespace
kubectl get <resource> --all-namespaces   # In all namespaces
kubectl get <resource> -o wide            # Add extended information
kubectl get <resource> -o yaml            # output in YAML format
kubectl get <resource> -o json            # output in JSON format

# Example
kubectl get pods [-n abc|--all-namespaces] [-o wide|yaml|json]

```

See:
[kubectl - Cheat Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)
for a more extended overview of the `kubectl` command.


[docker-desktop]: https://docs.docker.com/desktop/
[kind]: https://kind.sigs.k8s.io/
[minikube]: https://minikube.sigs.k8s.io/docs/
[autocompletion]:
  https://kubernetes.io/docs/tasks/tools/install-kubectl/#enabling-shell-autocompletion
