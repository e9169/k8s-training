# Replicas and Rolling update

## Create Deployment

If you have deleted the deployment we have been testing, install it again with the following command:

- `kubectl apply -f deployment.yaml`
- Increase the replicas to four in the deployment yaml file:

```yaml
spec:
  replicas: 3
```

- Apply the deployment again to increase the replica count of your deployment.

## Update Deployment

Now we will try to roll out an update to the image.

- Set image tag to `1.16.1`:

```yaml
    ...
    spec:
      containers:
      - image: nginx:1.16.1
```

- Apply the new version of your deployment.

- Check the rollout status: `kubectl rollout status deployment nginx`

- Investigate rollout history: `kubectl rollout history deployment nginx`

- Try also rolling out a version that does not exist:

```yaml
    spec:
      containers:
      - image: nginx:not-a-version
        name: nginx
```

What happened?

- Investigate the running pods with: `kubectl get pods`

## Undo Update

The rollout above using a non-existing image version caused some pods to be
non-functioning. Next, we will undo this faulty deployment.

- Investigate rollout history: `kubectl rollout history deployment nginx`

- Undo the rollout and restore the previous version: `kubectl rollout undo deployment nginx`

- Investigate the running pods: `kubectl get pods`

## Clean up

Delete deployments and services as follow:

- `kubectl delete deployment nginx`
- `kubectl delete service nginx`
