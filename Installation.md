# Kubernet installation step by step 
. Update the repository on both servers 
```
apt-get update
```
Disable the SWAP partition on both server 
```
sudo sed -i '/swap/d' /etc/fstab
swapoff -a
```
verifing 
```
cat /etc/fstab
```
Set the Hostname 
```
sudo hostnamectl set-hostname <host name>
```
Update the hostname on both the servers (/etc/hosts)
```
vim /etc/hosts
Kubernet-master01
Kubernet-Worker01
```

- Note the IP address Should be static

- Reboot the servers 


- Install Docker on both server 
```
sudo apt update
sudo apt install docker.io
sudo systemctl start docker
sudo systemctl enable docker
```

- Run the following command to run before installing the kubernet 
```
 sudo apt install apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add
sudo apt-add-repository "deb http://apt.kubernetes.io/ kubernetes-xenial main"
cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF
```

-  

```
mkdir -p /etc/systemd/system/docker.service.d

systemctl daemon-reload
systemctl restart docker
```

- Install kubeadm kubelet & kubectl on Both the server 
```
sudo apt install kubeadm kubelet kubectl kubernetes-cni
```
```
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add -
$ echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" | tee /etc/apt/sources.list.d/kubernetes.list
$ apt-get update
$ apt-get install -y kubelet=1.26.0-00 kubeadm=1.26.0-00 kubectl=1.26.0-00
```


- Run these on the master node: 
```
 sudo kubeadm init
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

- Install network plugin on Master
```
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

watch kubectl get pods --all-namespaces
kubectl get nodes -o wide
```

kubernet label name if it showing none you can change 
```
kubectl get node --show-labels

kubectl label node kubernet-worker01 node-role.kubernetes.io/worker01=
```

```
kubectl get node
```

genertae new key for join the kubeclsuter 
```
kubeadm token create --print-join-command
```




<h3 align="left">Connect with Me:</h3>
<a href="https://linkedin.com/in/Shyjustack" target="blank"><img align="center" src="https://raw.githubusercontent.com/rahuldkjain/github-profile-readme-generator/master/src/images/icons/Social/linked-in-alt.svg" alt="cloudnloud" height="30" width="40" /></a>
