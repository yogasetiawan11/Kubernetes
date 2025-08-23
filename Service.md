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
