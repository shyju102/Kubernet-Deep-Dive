# Kubernet Deployment 
Deployment sits top on the Kubernetes hierarchy layer. First Pod, then Replica Set, and then Deployment. By using Deployment we can easily upgrade the Pod instances using rolling update and we can easily undo the changes. So Deployment in the Kubernetes cluster is very flexible and gives us more value

<img width="744" alt="image" src="https://user-images.githubusercontent.com/62458394/158771686-15f3779c-ec5c-4e72-ba01-547910815ce3.png">

# Advantages of Deployment in Kubernetes
1. We can easily scale the application by creating multiple instances of a single Pod.
2. Upgrade the instances seamlessly. The Deployment uses the rolling update concept. The rolling update will update the instance one after another instead of updating all.
3. If the updated instances had some error, then we can easily undo the changes by rolling back to the previous version!

# How Deployment Works
1. First it will create a Deployment. Just a registry for the Pod definition.
2. Then the replica set is created for the Pod definition.
3. Then the desired number of Pods are created using the replica set.
- Two type of deployment strategy
1)	Destroy all of them and create new version 
![image](https://user-images.githubusercontent.com/62458394/158963332-61fbcb65-a496-4abf-8a22-44a0d2250a09.png)

- Note: During the upgradation time application is not available
This is not the default deployment strategy 

2) Do not destroy all of them - this is the default deployment 
Take down the older version and bring up a newer version one by one 
![image](https://user-images.githubusercontent.com/62458394/158965269-87ab0264-b7ee-4da8-ac25-7e1bcf944aaf.png)


# Create Deployment YAML File
The Deployment object in the Kubernetes cluster uses the same structure as Replica Set. Only the kind is different. Look at the below code, which is used to create a NodeJS application Deployment.

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nodeapp-deployment
  labels:
     app: nodeapp
     type: front-end
spec:
  template:
     metadata:
       name: nodejsapp-pod
       labels: 
         app: nodejsapp
         type: front-end
     spec:
         containers:
           - name: nodejsapp-erp
             image: nginx:1.16.1
  replicas: 3
  selector:
    matchLabels:
      type: front-end
```
- Testing 
```
 kubectl get deploy
 kubectl get rs
 kubectl get pod 
```
- If you want to upgrade the nginx verison 
```
kubectl set image deployment/nodeapp-deployment  nodejsapp-erp=nginx:1.19.1
```
- If you want to upgrade version trough Yaml file, modify the version in the file and use this command "kubectl apply -f <file name.yal>


- How to create the deployment on command line 
```
kubectl create deployment test-deploy --image=nginx --replicas=3
kubectl get deployments
kubectl set image deployment/test-deploy nginx=nginx:1.16.1
kubectl describe deploy test-deploy
kubectl set image deployment/test-deploy nginx=nginx:1.19.1
kubectl rollout undo deployment/test-deploy
kubectl rollout status deployment/test-deploy
kubectl rollout history deployment/test-deploy
kubectl rollout history deployment/test-deploy --revision=2
kubectl rollout undo deployment/test-deploy --to-revision=2
```

  # Thank you 
