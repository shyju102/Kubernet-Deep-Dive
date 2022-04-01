# 1. What are the features of Kubernetes?
<img width="572" alt="image" src="https://user-images.githubusercontent.com/62458394/161252840-c76d4a12-9da5-4e34-86ca-d20634c18f31.png">

# 2. What is Minikube?
Minikube is a tool that makes it easy to run Kubernetes locally. This runs a single-node Kubernetes cluster inside a virtual machine

# 3. What are the various K8's services running on nodes and describe the role of each service?
Mainly K8 cluster consists of two types of nodes, Worker and master.

- Worker node: (This runs on master node)

Kube-proxy: This service is responsible for the communication of pods within the cluster and to the outside network, which runs on every node. This service is responsible to maintain network protocols when your pod establishes a network communication.
kubelet: Each node has a running kubelet service that updates the running node accordingly with the configuration(YAML or JSON) file. NOTE: kubelet service is only for containers created by Kubernetes.
- Master Node:

Kube-apiserver: Master API service which acts as an entry point to K8 cluster.
Kube-scheduler: Schedule PODs according to available resources on executor nodes.
Kube-controller-manager:  is a control loop that watches the shared state of the cluster through the apiserver and makes changes attempting to move the current state towards the desired stable state

# 4. What are the various things that can be done to increase Kubernetes security?
By default, POD can communicate with any other POD, we can set up network policies to limit this communication between the PODs.

RBAC (Role-based access control) to narrow down the permissions.
Use namespaces to establish security boundaries.
Set the admission control policies to avoid running the privileged containers.
Turn on audit logging.
# 5.  How to monitor the Kubernetes cluster?
Prometheus is used for Kubernetes monitoring. The Prometheus ecosystem consists of multiple components.

Mainly Prometheus server which scrapes and stores time-series data.
Client libraries for instrumenting application code.
Push gateway for supporting short-lived jobs.
Special-purpose exporters for services like StatsD, HAProxy, Graphite, etc.
An alert manager to handle alerts on various support tools

# 6. How to get the central logs from POD?
This architecture depends upon the application and many other factors. Following are the common logging patterns

Node level logging agent.
Streaming sidecar container.
Sidecar container with the logging agent.
Export logs directly from the application.

# 7. What is the difference between Docker Swarm and Kubernetes?

The installation procedure of the K8s is very complicated but if it is once installed then the cluster is robust. On the other hand, the Docker swarm installation process is very simple but the cluster is not at all robust.
Kubernetes can process the auto-scaling but the Docker swarm cannot process the auto-scaling of the pods based on incoming load.
Kubernetes is a full-fledged Framework. Since it maintains the cluster states more consistently so autoscaling is not as fast as Docker Swarm.

# 8. How to run a POD on a particular node?
Various methods are available to achieve it.

nodeName: specify the name of a node in POD spec configuration, it will try to run the POD on a specific node.
nodeSelector: Assign a specific label to the node which has special resources and use the same label in POD spec so that POD will run only on that node.
nodeaffinities: required DuringSchedulingIgnoredDuringExecution, preferredDuringSchedulingIgnoredDuringExecution are hard and soft requirements for running the POD on specific nodes. This will be replacing nodeSelector in the future. It depends on the node labels

# 9. 
