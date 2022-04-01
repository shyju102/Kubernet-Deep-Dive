# 1. What are the features of Kubernetes?
<img width="572" alt="image" src="https://user-images.githubusercontent.com/62458394/161252840-c76d4a12-9da5-4e34-86ca-d20634c18f31.png">

# 2. What is Minikube?
Minikube is a tool that makes it easy to run Kubernetes locally. This runs a single-node Kubernetes cluster inside a virtual machine

# 3. What are the various K8's services running on nodes and describe the role of each service?
Mainly K8 cluster consists of two types of nodes, Worker and master.

Worker node: (This runs on master node)

Kube-proxy: This service is responsible for the communication of pods within the cluster and to the outside network, which runs on every node. This service is responsible to maintain network protocols when your pod establishes a network communication.
kubelet: Each node has a running kubelet service that updates the running node accordingly with the configuration(YAML or JSON) file. NOTE: kubelet service is only for containers created by Kubernetes.
Master services:

Kube-apiserver: Master API service which acts as an entry point to K8 cluster.
Kube-scheduler: Schedule PODs according to available resources on executor nodes.
Kube-controller-manager:  is a control loop that watches the shared state of the cluster through the apiserver and makes changes attempting to move the current state towards the desired stable state
