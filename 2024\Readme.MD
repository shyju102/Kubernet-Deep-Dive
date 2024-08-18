# Kubernetes on Ubuntu 24.04 (Step-by-Step Guide)

## Prerequisites
- Pre-Install Ubuntu 24.04 Instances
- SSH enabled on all the Instances
- Regular User with sudo rights
- Minimum of 2GB RAM, 2 CPUs and 20 GB free disk space on each instance
- Stable Internet Connectivity

## Disable Swap and Load Kernel Modules
It is highly recommended to disable swap space on your Ubuntu instances so that Kubernetes cluster works smoothly. Run beneath command on each instance to disable swap space.

```
sudo swapoff -a
```
```
sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```

Now, load the following kernel modules using modprobe command.

```
sudo modprobe overlay
```

```
sudo modprobe br_netfilter
```

For the permanent loading of these modules, create the file with following content.


```
sudo tee /etc/modules-load.d/k8s.conf <<EOF
overlay
br_netfilter
EOF
```

Next, add the kernel parameters like IP forwarding. Create a file and load the parameters using sysctl command,
```
sudo tee /etc/sysctl.d/kubernetes.conf <<EOT
net.bridge.bridge-nf-call-ip6tables = 1
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
EOT
```

To load the above kernel parameters, run
```
sudo sysctl --system
```

## Install and Configure Containerd
Containerd provides the container run time for Kubernetes. So, Install containerd on all three instances.

First install containerd dependencies,
```
sudo apt install -y curl gnupg2 software-properties-common apt-transport-https ca-certificates
```
Next, add containerd repository using following set of commands.
```
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmour -o /etc/apt/trusted.gpg.d/containerd.gpg
```
```
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```
Now, install containerd using following apt command.

```
sudo apt update && sudo apt install containerd.io -y
```
Next, configure containerd so that it starts using SystemdCgroup. Run beneath commands.
```
containerd config default | sudo tee /etc/containerd/config.toml >/dev/null 2>&1
```
```
sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' /etc/containerd/config.toml
```
Restart containerd service so that above changes come into the affect.

```
sudo systemctl restart containerd
```   
