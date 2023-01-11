# Kubernetes storage (PV and PVC):

Let's start by getting into the node and create a simple html file:

`minikube ssh`

Once inside the node run:

`sudo mkdir /mnt/data`

Here, create an index.html file:

`sudo sh -c "echo 'Kubernetes storage test' > /mnt/data/index.html"`

Close the shell and let's continue with the persistent volume and persistent volume claim definitions.

First, list the available storage classes. This is important since defines which kind of storage is available on the cluster.

```shell
kubectl get storageclass
```

Then use the `05-storage-pvc.yaml` manifest to create a persistent volume and a persistent volume claim.

```shell
kubectl apply -f 05-storage-pvc.yaml
```

After creating both objects, the Kubernetes control plane looks for a PersistentVolume that satisfies the claim. If it finds a suitable PersistentVolume with the same StorageClass, it binds the claim to the volume.

Check that the PVC exists and is bound:

```shell
kubectl get pvc
```

Take also a look at the PV:

```shell
kubectl get pv
```


Expected output:

```shell
NAME        STATUS    VOLUME                                     CAPACITY   ACCESS MODES   STORAGECLASS   AGE
pvc-nginx   Bound     pvc-xxxx-xxxx-xxxx-xxxx-xxxx   1Gi        RWO            standard       4m
```

> NB: if it's not bound, try to `describe` it and see the message?
>
> Notice the output from `kubectl get storageclass`, if it says
> `VOLUMEBINDINGMODE=WaitForFirstConsumer`
> then you need a Pod or Deployment to "use" it first.

Let's use that volume!

```yaml,k8s
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      volumes:
      - name: pv-storage
        persistentVolumeClaim:
          claimName: pvc-nginx
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
        volumeMounts:
        - mountPath: "/usr/share/nginx/html"
          name: pv-storage
```

Save this code to nginx-storage.yaml and deploy nginx again.

```
kubectl apply -f nginx-storage.yaml
```

After it starts, you may want to examine it by using `kubectl describe pod nginx` and look out for volume declarations.

Let's try our volume configuration.

`kubectl exec -it nginx-xxxx-xxxx -- /bin/bash`

Once in the pod shell, let's verify that nginx is serving our index.html file from the volume.

```shell
apt update
apt install curl
curl http://localhost/
```

## Clean up

```
kubectl delete deployment nginx-deployment
kubectl delete service nginx-test
kubectl delete pvc pvc-nginx
```
