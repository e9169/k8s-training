# Setup Namespace

Namespaces are the default way for kubernetes to separate resources.
    They do not share anything between them.

## 1.1 Create a namespace

Namespaces are resources themselves, so they can be created like any other resource. Choose a name for your namespace:

```shell
$ kubectl create namespace my-namespace
namespace "my-namespace" created
```

## 1.2 Scoping the kubectl command

You want to target your own namespace instead of default one every time you use `kubectl`.
    You can run a command in a specific namespace by using the `-n, --namespace=''`-flag.

The below commands do the same thing, because kubernetes commands will default to the default namespace:

```shell
kubectl get pods -n default
kubectl get pods
```

## 1.3 Set your default namespace

Writing this every time you want to select your own namespace can be boring,
    so it makes sense to set this as the default.

To overwrite the default namespace for your current `context`, run:

```shell
$ kubectl config set-context $(kubectl config current-context) --namespace=my-namespace
Context "<your current context>" modified.
```

You can verify that you've updated your current `context` by running:

```shell
kubectl config get-contexts
```

Notice that the namespace column has the value of `<my-namespace>`.

You can also use `kubens`, which is an addon for easy management of the working namespace in a cluster.

## 1.4 More on Namespaces

Namespaces are quite powerful. On a user level, it is also possible to limit namespaces and resources by users but this is a bit too involved for your first experience with Kubernetes.

Kubernetes clusters come with a namespace called `default`, and usually one called `kube-system` which will contain some of the kubernetes services running in the cluster.

You might see later that the namespace is specified directly in the yaml files describing the resources.
    This makes it possible to have the resource created in the specific namespace without specifying the `-n` flag on creation.
