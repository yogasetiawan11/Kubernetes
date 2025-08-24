# What is service in Kubernetes
A Kubernetes Service is an abstraction layer which defines a logical set of Pods and enables external traffic exposure, load balancing and service discovery for those Pods.

When you create, destroy and replace the Pods come up with Their Default IP adresses all of the IP addresses are not stable (Always change), Because of this service acs as an abstraction layer that defines a logical set of pods and a policy to access them.

Service offers your 3 advantages that is Load Balancing, Service Discovery, Expose your app to the World

## Load Balancing

<img width="1000" height="1000" alt="Image" src="https://github.com/user-attachments/assets/7e41b806-effa-45c6-a3d5-38cae2ca1684" />

When you create a Deployment that contain your Pods using Replica set on top of this you'll create a **Service** And This Service will access a Load Balancer, It uses a component In kubernetes called as **Kube Proxy**

If there are visitor accessing the The Pods (application) Instead you can create a service and give the visitors access to access the service 

## Kubernetes Service Discovery

In the world Kubernetes It's commonly Pods in kubernetes Goes down, If this thing happened Kubernetes will create new Pods. But the Problem is that Without service mechanism K8s will create a New Pods with different IP addresses.

So to solve this Problem service can keep track of all of the Pods with **Label** & **Selector** Mechanism. Whenever there's a Pods that is created Devops or Developer have to create *Label* for each Pods. If the container goes down and Pods will come up with new IP address but the *Label* will remain the same. Because the replica set controller It will deploy the same part with the same Yaml that is Auto Healing. So this is How Serviece Mechanism of Kubernetes work.

## Expose To World
There are 3 default types of sevice you can create whithin yaml manifest. each service have different Purpose:
1. Cluster IP
2. Node Port
3. Load Balancer

### Cluster IP
If you create a service using Cluster IP model, It's default behaviour, so your application will be still accessed inside Kubernetes cluster.

With this mode you will have 2 advantages that is, Discovery and Load Balancing.

### Node Port 
With this mode It will allow to be accessed inside your Organization or your Network.

So whoever has access to your Worker Node IP addresses they can access your application If you create service as type Node Port

### Load Balancer
This mode will expose you application to external world 

let's say you have deployed your app inside EKS, If you create a Load Balancer service then you will get an **Elastic Load balancing** IP address for your specific service so with this IP address anyone can access your app. 

So anyone can access this sevice because It is created by Public IP. 

So If there were no services concept in Kubernetes you would fail terribly even you have Auto Healing capabilty or even you have Deployment and your Application won't work for certain people, when the Application goes down It comes up with a new IP address, This is the Problem that Service solved.


# Practical example
A Step-by-Step Guide: Docker Image to Kubernetes Service
This guide provides a clear, step-by-step process for deploying an application to Kubernetes. We will cover building a Docker image, pushing it to a registry, and then using Kubernetes manifests to create a Deployment and a Service.

## Step 1: Prepare Your Application
Before you containerize application, you need to have one ready. In This example I create simple Python web application that listens on a specific port.

## Step 2: Create a Dockerfile
A Dockerfile is a script that contains a series of instructions to build a Docker image. I specify the base image, copies your application code, installs dependencies, and also defines the command to run the application.

This is my Docker file.
```bash
FROM ubuntu

WORKDIR /app

COPY requirements.txt /app/
COPY devops /app/

RUN apt-get update && apt-get install -y python3 python3-pip python3-venv

SHELL ["/bin/bash", "-c"]

RUN python3 -m venv venv1 && \
source venv1/bin/activate && \
pip install --no-cache-dir -r requirements.txt

EXPOSE 8000

CMD source venv1/bin/activate && python3 manage.py runserver 0.0.0.0:8000
```

## Step 3: Build the Docker Image
Once my Dockerfile is ready, I can build the image using the docker build command. 

```bash
docker build -t yogas4/python-app:v1 .
```

## Step 4: Push the Image to a Registry
A Docker registry (like Docker Hub or a private registry) is a centralized location for storing and distributing Docker images. Once you create the Docker image next Pushing the image to a registry makes it accessible for your Kubernetes cluster to pull and run.

First, log in to your registry:
```bash 
docker login
```
Then, push the image you just built:
```bash
docker push yogas4/python-app:v1
```

## Step 5: Create a Kubernetes Deployment
A Deployment is a Kubernetes object that manages a set of identical Pods. It ensures that a specified number of replicas of your application are running and handles updates and scaling. Create a file named deployment.yaml with the following content:

## my deployment.yaml
```bash
apiVersion: apps/v1
kind: Deployment
# Ensure the name of label and selector are same
metadata:
  name: python-application
  labels:
    app: python-application
spec:
  replicas: 2
  selector:
    matchLabels:
      app: python-application
  template:
    metadata:
      labels:
        app: python-application
    spec:

      containers:
      - name: python-app
        image: yogas4/python-app:v1
        ports:
        # attach with app's Port
        - containerPort: 8000
```

Apply the script with:
```bash
kubectl apply -f deployment.yaml
```
```bash
kubectl get deploy
```
The result:
```bash
NAME                 READY   UP-TO-DATE   AVAILABLE   AGE
python-app           2/2     2            2           34m
python-application   2/2     2            2           9m29s
```
## Step 6: Create a Kubernetes Service
A Service is a Kubernetes object that defines a logical set of Pods and a policy to access them. It provides a stable IP address and DNS name, acting as a load balancer for your application.

Create a file named service.yaml with the following content. This example uses NodePort, which exposes the service on each node's IP at a static port.

# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app-service
spec:
  type: NodePort # Other types include ClusterIP and LoadBalancer
  selector:
    app: my-app # This links the service to your deployment's pods via the label
  ports:
    - protocol: TCP
      port: 80 # The service's port
      targetPort: 3000 # The container's port
      nodePort: 30000 # A static port on each node (optional, for NodePort type)

Step 7: Apply the Kubernetes Manifests
With your YAML files ready, use the kubectl apply command to create the Deployment and Service on your cluster.

kubectl apply -f deployment.yaml
kubectl apply -f service.yaml

Step 8: Verify and Access the Service
You can check the status of your resources with kubectl get commands:

# Check the deployment and its pods
kubectl get deployments
kubectl get pods

# Check the service
kubectl get services

To access your application, find the NodePort assigned to your service (if you used NodePort). The output of kubectl get services will show the port mapping (e.g., 80:30000/TCP). You can then access the app at http://<your-node-ip>:30000.