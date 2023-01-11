# Service Discovery

## Accessing a service

To access any service inside any given pod (e.g. nginx web service), we need to _expose_ the related deployment as a _service_. We have three main ways of exposing the deployment , or in other words, we have three ways to define a _service_ , which we can access in three different ways. A service is usually created on top of an existing deployment.

### Service type: ClusterIP

Services of type ClusterIP will only create a DNS entry with the name of the service, as well as an internal cluster IP that routes traffic over to the deployments hit by the `selector` part of the service.

The service type ClusterIP does not have any external IP. This means it is not accessible over internet, but we can still access it from within the cluster using its `CLUSTER-IP`.

### Service type: NodePort

A service type NodePort creates a port on a node that is reachable from the outside. Notice that we still don't have an external IP, but we now have an extra port e.g. `32593` for this service.

This port is a **NodePort** exposed on the worker nodes. So now, if we know the IP of our nodes, we can access this service from the internet.

### Service type: LoadBalancer

The type LoadBalancer is only available for use, if your k8s cluster is setup in any of the public cloud providers, GCE, AWS, etc or that the admin of a local cluster have set it up, so we will skip this for this training.


### Service definition

We can create a service for our nginx deployment with the following yaml definition:

```yaml
apiVersion: v1
kind: Service
metadata:
  labels:
    app: nginx-test
  name: nginx-test
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    app: nginx-test
  type: ClusterIP
```

Test it with `minikube service nginx-test`. Your browser should open a new tab pointing to the service!
