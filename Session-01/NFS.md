In this post, let’s see how to present the NFS file mount to the Kubernetes cluster running on Ubuntu.

 If you have not yet checked the previous parts of this series, please go ahead and check this  👉 [Link](https://blog.cloudnloud.com/series/kubernetes-deep-dive)

# NFS Volume In Kubernetes 
Kubernetes Volumes are abstracted storage units that allow nodes within a cluster to write, read and share data between them. Kubernetes offers many storage plugins that provide access to storage services and platforms. One of these is the NFS

## There are two ways to access data via NFS in Kubernetes:

**Persistent Volume with NFS** - this lets you set up a managed resource within the cluster that is accessed via NFS. 

**Directly map the NFS volume to  the pod**


Kubernetes allows you to mount a Volume as a local drive on a container.

## Advantages of Using NFS with Kubernetes

**Share data -** because of its persistent nature, NFS Volumes can be used to share data between containers, whether in the same pod or different pods.


## Step 1: 
**Requirement: ** NFS Server

Please refer to this link  👉  : [NFS Server Configuration ](https://blog.cloudnloud.com/nfs-server-and-client-configuration) 

**Diagram **


![Shyju-Kubernetes (6).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660797255165/y2q3WUU5G.png align="left")

### PV creation 
```
root@Kubernet-Master:~# cat pv.yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: nfs-pv
  labels:
    name: mynfs # name can be anything
spec:
  storageClassName: manual # same storage class as pvc
  capacity:
    storage: 200Mi
  accessModes:
    - ReadWriteMany
  nfs:
    server: 192.168.100.102 # NFS server IP address 
    path: "/mnt/nfs_share"   # NFS share location 
root@Kubernet-Master:~#
```
**Check the PV status **
```
root@Kubernet-Master:~# kubectl get pv
NAME     CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM             STORAGECLASS   REASON   AGE
nfs-pv   200Mi      RWX            Retain           Bound    default/nfs-pvc   manual                  41h

``` 

### PVC creation 
```
root@Kubernet-Master:~# cat pvc.yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: nfs-pvc
spec:
  storageClassName: manual
  accessModes:
    - ReadWriteMany #  must be the same as PersistentVolume
  resources:
    requests:
      storage: 50Mi

``` 
**Check the PVC status **
```
root@Kubernet-Master:~# kubectl get pvc
NAME      STATUS   VOLUME   CAPACITY   ACCESS MODES   STORAGECLASS   AGE
nfs-pvc   Bound    nfs-pv   200Mi      RWX            manual         41h
root@Kubernet-Master:~#

``` 
### Deployment creation with PVC 
```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx
  name: nfs-nginx
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
      - name: nfs-test
        persistentVolumeClaim:
          claimName: nfs-pvc # same name of pvc that was created
      containers:
      - image: nginx
        name: nginx
        volumeMounts:
        - name: nfs-test # name of volume should match claimName volume
          mountPath: /usr/share/nginx/html # mount inside of contianer

```
 Create the Deployment 

```
root@Kubernet-Master:~# kubectl create -f deployment.yaml
deployment.apps/nfs-nginx created
``` 

 ### Testing the Pod status 
```
root@Kubernet-Master:~# kubectl get pod
NAME                         READY   STATUS    RESTARTS        AGE
nfs-nginx-5c89779fb6-v8lnv   1/1     Running   0               59m
``` 
### Testing the volume status 
```
root@Kubernet-Master:~# kubectl exec -it nfs-nginx-5c89779fb6-v8lnv bash
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
root@nfs-nginx-5c89779fb6-v8lnv:/# df -h
Filesystem                      Size  Used Avail Use% Mounted on
overlay                          98G   13G   80G  14% /
tmpfs                            64M     0   64M   0% /dev
tmpfs                           957M     0  957M   0% /sys/fs/cgroup
shm                              64M     0   64M   0% /dev/shm
/dev/sda5                        98G   13G   80G  14% /etc/hosts
192.168.100.102:/mnt/nfs_share   98G   13G   80G  14% /usr/share/nginx/html
tmpfs                           1.8G   12K  1.8G   1% /run/secrets/kubernetes.io/serviceaccount
tmpfs                           957M     0  957M   0% /proc/acpi
tmpfs                           957M     0  957M   0% /proc/scsi
tmpfs                           957M     0  957M   0% /sys/firmware

``` 
**Volume is mapped under the pod **


## Step 2
**Requirement: ** NFS Server 

**Diagram **

![Shyju-Kubernetes (5).png](https://cdn.hashnode.com/res/hashnode/image/upload/v1660797122131/OnRA3dAi3.png align="left")

### Pod creation manifest file 
In the same file, mention the NFS server details and mount location.
```
kind: Pod
apiVersion: v1
metadata:
  name: nfs
spec:
  containers:
    - name: app
      image: httpd
      volumeMounts:
        - name: nfs-volume
          mountPath: /var/nfs # Please change the destination you like the share to be mounted too
  volumes:
    - name: nfs-volume
      nfs:
        server: 192.168.100.102  # NFS Server Ip address 
        path: /mnt/nfs_share  # NFS Server share location 

``` 
### Creating the pod 
```
root@Kubernet-Master:~# kubectl create -f NFSPOD.yaml
pod/nfs created
``` 
### POD status 
```
root@Kubernet-Master:~# kubectl get pod
NAME   READY   STATUS    RESTARTS   AGE
nfs    1/1     Running   0          82s
root@Kubernet-Master:~#

``` 
### Verifying the NFS mount path 

```
root@Kubernet-Master:~# kubectl exec -it nfs bash
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
root@nfs:/usr/local/apache2# df -h
Filesystem                      Size  Used Avail Use% Mounted on
overlay                          98G   13G   80G  14% /
tmpfs                            64M     0   64M   0% /dev
tmpfs                           957M     0  957M   0% /sys/fs/cgroup
192.168.100.102:/mnt/nfs_share   98G   13G   80G  14% /var/nfs
dev/sda5                        98G   13G   80G  14% /etc/hosts
shm                              64M     0   64M   0% /dev/shm
tmpfs                           1.8G   12K  1.8G   1% /run/secrets/kubernetes.io/serviceaccount
tmpfs                           957M     0  957M   0% /proc/acpi
tmpfs                           957M     0  957M   0% /proc/scsi
tmpfs                           957M     0  957M   0% /sys/firmware
root@nfs:/usr/local/apache2#
