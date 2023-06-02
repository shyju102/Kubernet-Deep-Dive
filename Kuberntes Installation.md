Disable swap
```swapoff -a; sed -i '/swap/d' /etc/fstab```
Disable Firewall
```systemctl disable --now ufw```
Install containerd runtime
```{
  apt update
  apt install -y containerd apt-transport-https
  mkdir /etc/containerd
  containerd config default > /etc/containerd/config.toml
  systemctl restart containerd
  systemctl enable containerd
}```

Add apt repo for kubernetes
```{
  curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
  apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
}```
Install Kubernetes components
```{
  apt update
  apt install -y kubeadm kubelet kubectl
}```
On kmaster1
Initialize Kubernetes Cluster
```kubeadm init ```


Install Calico with Kubernetes API datastore, 50 nodes or less
Download the Calico networking manifest for the Kubernetes API datastore.

```curl https://raw.githubusercontent.com/projectcalico/calico/v3.26.0/manifests/calico.yaml -O```

If you are using pod CIDR 192.168.0.0/16, skip to the next step. If you are using a different pod CIDR with kubeadm, no changes are required - Calico will automatically detect the CIDR based on the running configuration. For other platforms, make sure you uncomment the CALICO_IPV4POOL_CIDR variable in the manifest and set it to the same value as your chosen pod CIDR.
Customize the manifest as necessary.

Apply the manifest using the following command.

```kubectl apply -f calico.yaml```


Verifying the cluster
```kubectl cluster-info```
```kubectl get nodes``

#Thanks!
