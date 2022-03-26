# We get three ways to create pods from the kubectl CLI
- kubectl run (changing to be only for pod creation)
- kubectl create (create some resources via CLI or YAML)
- kubectl apply (create/update anything via YAML)

# Creating Pods with kubectl

```
kubectl run   <POD Name> --image=nginx
```
Copy the instruction  to file.yaml
```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx:1.14.2
    ports:
    - containerPort: 80

```
<img width="424" alt="basic-pod-manifest-nginx" src="https://user-images.githubusercontent.com/62458394/160065148-01fa4abb-9472-43d2-8ab7-0f63ed805351.PNG">


![image](https://user-images.githubusercontent.com/62458394/160065362-b0c745cb-f813-4f5e-845a-1b446886e7c8.png)


```
kubectl create -f file.yaml
```
How to list the pod 
```
kubectl get pod 
```
- List all the objects 
```
kubectl get all
```
How to delete the pod 
```
kubectl delete pod <POD Name>
```
# Namespaces
- In Kubernetes, namespaces provides a mechanism for isolating groups of resources within a single cluster. Names of resources need to be unique within a namespace, but not across namespaces. 
- <img width="283" alt="image" src="https://user-images.githubusercontent.com/62458394/159252984-4ace434b-6a90-4c55-a1f6-ac72a7d22c80.png">


You can list the current namespaces in a cluster using:
```
kubectl get namespace
```
- Create namespace trough manifest file
```
apiVersion: v1
kind: Namespace
metadata:
  name: myname
```

- Create Namespace trough CLI 
```
kubectl create ns test
```
- How to create the container inside the test namespace 
```
kubectl run nginx -n test --image=nginx
```
```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  namespace: test
  labels:
    run: nginx
  name: nginx10
spec:
  containers:
  - image: nginx
    name: nginx

```
