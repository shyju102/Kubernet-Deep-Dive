### What is the Ingress?

The Ingress is a Kubernetes resource that lets you configure an HTTP load balancer for applications running on Kubernetes, represented by one or more Service. Such a load balancer is necessary to deliver those applications to clients outside of the Kubernetes cluster.

The Ingress resource supports the following features:
* **Content-based routing**:
    * *Host-based routing*. For example, routing requests with the host header `foo.example.com` to one group of services and the host header `bar.example.com` to another group.
    * *Path-based routing*. For example, routing requests with the URI that starts with `/serviceA` to service A and requests with the URI that starts with `/serviceB` to service B.
* **TLS/SSL termination** for each hostname, such as `foo.example.com`.

See the [Ingress User Guide](https://kubernetes.io/docs/user-guide/ingress/) to learn more about the Ingress resource.

## The Ingress Controller at a High Level

![ingress](https://user-images.githubusercontent.com/62458394/166744986-6af3dc87-787a-422b-998f-8acab6cdb7d7.jpg)

### What is the Ingress Controller?
The Ingress Controller is an application that runs in a cluster and configures an HTTP load balancer according to Ingress resources. The load balancer can be a software load balancer running in the cluster or a hardware or cloud load balancer running externally. Different load balancers require different Ingress Controller implementations.

In the case of NGINX, the Ingress Controller is deployed in a pod along with the load balancer.
