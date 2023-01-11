# Declarative deployment

## Deploy an application using a declarative configuration file

Although the `kubectl create` command allows us to specify a number of flags to
configure which kind of deployment should be created, it's easier to work
with declarative deployment in a _deployment spec_-file and `create` that
instead.

To create the `nginx` you can use the provided `yaml` file below.

The contents are as follows:

```yaml
# a comment
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx # arbitrary label on deployment
spec:
  replicas: 1
  selector:
    matchLabels: # labels the replica selector should match
      app: nginx
  template:
    metadata:
      labels:
        app: nginx # label for replica selector to match
        version: 1.14.2 # arbitrary label we can match on elsewhere
    spec:
      containers:
        - name: nginx
          image: nginx:1.14.2
          ports:
            - containerPort: 80
```

To create one or more objects specified by a file, run:

```
kubectl apply -f nginx-deployment.yaml
```

Expected output:

```
deployment.apps/nginx created
```



Verify that the deployment is created:

```
kubectl get deployments
```

Expected output:

```
NAME        READY   UP-TO-DATE   AVAILABLE   AGE
nginx       1/1     1            1           36s
```

Check if the pods are running:

```
kubectl get pods
```

Expected output:

```
NAME                         READY     STATUS    RESTARTS   AGE
nginx-xxxx-xxxx        1/1       Running   0          40s
```

## Test Kubernetes resilience by deleting a pod

> You might think that now if you delete the pod you will be finished with it, but what happens reallly?
> That a new pod is created in its place.

Let's see this in action:

```
kubectl delete pod nginx-xxxx-xxxx
```

Expected output:

```
pod "nginx-xxxx-xxxx" deleted
```

As soon as we delete a pod, a new one is created, satisfying the desired state
by the deployment, which is - it needs at least one pod running nginx.

So rightfully we see that a **new** nginx pod is created (with a new name):

```
kubectl get pods
```

Expected output:

```
NAME                         READY     STATUS              RESTARTS   AGE
nginx-xxxx-xxxx        0/1       ContainerCreating   0          5s
```

.. and after few more seconds:

```
kubectl get pods
```

Expected output:

```
NAME                         READY     STATUS    RESTARTS   AGE
nginx-xxxx-xxxx        1/1       Running   0          12s
```

## Clean up

Delete the `nginx` deployment:

```
kubectl delete deployment nginx
```
