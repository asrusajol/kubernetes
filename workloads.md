### Creating a Deployment (Imperative)
```
kubectl create deployment redis-deployment --image=redis
```
### Specifying Additional Parameters (Imperative)
```
kubectl create deployment nginx-deployment --image=nginx --replicas=3 
```
### Verifying the Deployment 
```
kubectl get deployments
```
### Viewing Deployment Details
```
kubectl describe deployment nginx-deployment
```
### Create YAML menifest file from the imperative command

```sh
kubectl create deployment nginx-deployment --image=nginx --dry-run=client -o yaml > nginx-deployment.yaml
```

### Creating a Deployment with YAML (Declarative)
```
create a YAML file (nginx-deployment.yaml) with the following content:
```
``` 
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx-deployment
  name: nginx-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-deployment
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 25%
      maxSurge: 25%
  template:
    metadata:
      labels:
        app: nginx-deployment
    spec:
      containers:
      - image: nginx
        name: nginx
        resources:
          requests:
            memory: "64Mi"
            cpu: "250m"
          limits:
            memory: "128Mi"
            cpu: "500m"
status: {}
```
```
kubectl apply -f nginx-deployment.yaml
```
### Scaling Up Pods
```
kubectl scale deployment my-deployment --replicas=5
```
### Scaling Down Pods
```
kubectl scale deployment my-deployment --replicas=2
```
### kubectl scale deployment my-deployment --replicas=2
```
kubectl autoscale deployment my-deployment --min=1 --max=10 --cpu-percent=80
```
### Creating a Deployment with YAML (Declarative)
```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: nginx-deployment
  name: nginx-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-deployment
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 25%
      maxSurge: 25%
  template:
    metadata:
      labels:
        app: nginx-deployment
    spec:
      containers:
      - image: nginx
        name: nginx
        resources:
          requests:
            memory: "64Mi"
            cpu: "250m"
          limits:
            memory: "128Mi"
            cpu: "500m"
status: {}
```
### Checking the Status
```
kubectl get deployment my-deployment
```
### To get detailed information about the HPA
```
kubectl get hpa
```


 
