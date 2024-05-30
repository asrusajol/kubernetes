### Creating a Deployment (Imperative)
```
kubectl create deployment nginx-deployment --image=nginx
```
### Specifying Additional Parameters (Imperative)
```
kubectl create deployment nginx-deployment --image=nginx --replicas=3 --port=80
```
### Verifying the Deployment 
```
kubectl get deployments
```
### Viewing Deployment Details
```
kubectl describe deployment nginx-deployment
```
### Creating a Deployment with YAML (Declarative)
```
create a YAML file (nginx-deployment.yaml) with the following content:
```
``` 
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
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
  name: nginx-deployment
spec:
  selector:
    matchLabels:
      run: nginx
  template:
    metadata:
      labels:
        run: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:latest
        ports:
        - containerPort: 80
        resources:
          limits:
            cpu: 500m
          requests:
            cpu: 200m
```
### Checking the Status
```
kubectl get deployment my-deployment
```
### To get detailed information about the HPA
```
kubectl get hpa
```


 
