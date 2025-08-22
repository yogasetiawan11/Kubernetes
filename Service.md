# What is service in Kubernetes
Service in kubernetes is a way to expose a set of Pods as a network service. It provides stable IP addresses and DNS name for the Pods.

When you create, destroy and replace the Pods Their IP adresses are not stable (Always change), Because of this service acs as an abstraction layer


There are 3 default types of sevice you can create whithin yaml manifest. each service have different Purpose:
1. Cluster IP
2. Node Port
3. Load Balancer
 
### Cluster IP
If you create a service using Cluster IP mode, this will be by default behaviour, so your application will be still accessed inside Kubernetes cluster.

With this mode you will have 2 advantages that is, Discovery and Load Balancing.

### Node Port 
With this mode It will allow to be accessed inside your Organization or your Network.

So whoever has access to your Worker Node IP addresses they can access your application If you create service as type Node Port

### Load Balancer
This mode will expose you application to external world 

let's say you have deployed your app inside EKS, If you create a Load Balancer service then you will get an **Elastic Load balancing** IP address for your specific service. 

So anyone can access this sevice because It is created by Public IP.
