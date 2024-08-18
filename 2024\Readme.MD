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
