# What is POD
A POD acts as a wrapper for one container or more. so POD can provide shared environment for them to run and communicate.

Shared Network: All containers within a Pod share the same network. so they can communicate with each other using localhost and share the same IP address and port space.

Shared Storage: Pods can have shared storage volumes that all containers in the Pod can access.

Encapsulation: The Pod encapsulates the container, providing a clear boundary for Kubernetes to manage. Instead of managing individual containers, Kubernetes manages Pods, which simplifies deployment, scaling, and networking.

# Deploy first Node
Create Yaml file then fill this script based on your requirement.
```bash
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
within this yaml file you can modify the value with your containers.

Then create the Pod followed this command:
```bash
kubectl create -f first-pod 
```

Output

```bash
pod/nginx created
```
It shows the Pod has created.

check status of the Pod 
```bash
kubectl get pods 
```
or get output more detail use this command
```bash
kubectl get pods -o wide 
```