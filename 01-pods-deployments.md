# Pods and Deployments

A **Pod** (_not container_) is the smallest building-block/worker-unit in Kubernetes,
it has a specification of one or more containers and exists for the duration of the containers;
if all the containers stop or terminate, the Pod is stopped.

Usually a pod will be part of a **Deployment**; a more controlled or _robust_ way of running Pods.
A deployment can be configured to automatically delete stopped or exited Pods and start new ones,
as well as run a number of identical Pods e.g. to provide high-availability.

So let's try to use some of the Kubernetes objects, starting with a deployment.

## Create deployments using `create` command

We start by creating our first deployment. Normally we will run a pod with a Nginx container as a first example.

Here is the command to do it:

```
kubectl create deployment nginx-deployment --image=nginx:1.14.2
```

Expected output:

```
deployment.apps/nginx-deployment created
```

So what happened? The `create`-command is great for imperative testing, and getting something up and running fast.
It creates a _[deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)_ named `nginx-deployment`, which creates a _[replicaset](https://kubernetes.io/docs/concepts/workloads/controllers/replicaset/)_, which starts a _[pod](https://kubernetes.io/docs/concepts/workloads/pods/)_ using the docker image `nginx:1.14.2`. You don't need to concern yourself with all these details at this stage, this is just extra but important information.

You can check the objects you've created with `get <object>`,
either one at a time, or all-together like below:

```
kubectl get deployment,replicaset,pod    # NB: no whitespace in the comma-separated list
```

Expected output:

```
NAME                              DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nginx-deployment   1         1         1            1           1m

NAME                                         DESIRED   CURRENT   READY     AGE
replicaset.apps/nginx-deployment-5c8676565d   1         1         1         1m

NAME                             READY     STATUS    RESTARTS   AGE
pod/nginx-deployment-xxxxx-xxxxx   1/1       Running   0          1m
```

> A ReplicaSet is something which deals with the number of copies of this pod.
> It will be covered in a later exercise, but it's mentioned and shown above for completeness.

## Clean up

Delete the `nginx-deployment` deployments:

```
kubectl delete deployment nginx-deployment
kubectl delete service nginx-deployment
```
