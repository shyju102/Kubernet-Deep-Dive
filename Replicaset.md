# What is ReplicaSet
A ReplicaSet's purpose is to maintain a stable set of replica Pods running at any given time.
<img width="400" alt="image" src="https://user-images.githubusercontent.com/62458394/159233071-08dc8dee-222c-4582-8057-45f2aabb3e80.png">


- Create Replicaset YAML File
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-proxy
  labels:
    app: nginx-proxy
    tier: frontend
spec:
  # modify replicas according to your case
  replicas: 3
  selector:
    matchLabels:
      tier: frontend
  template:
    metadata:
      labels:
        tier: frontend
    spec:
      containers:
      - name: nginx
        image: nginx
   ```
You can then get the current ReplicaSets deployed:
```
kubectl get rs
```
You can also check on the state of the ReplicaSet:
```
kubectl describe rs/<replica name>
```

