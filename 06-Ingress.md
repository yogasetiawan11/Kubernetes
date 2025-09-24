# What is Ingress

Ingress in Kubernetes is like a traffic controller for your cluster. Instead of use service stand alone to exposing each service with a NodePort or LoadBalancer, Ingress provides a central entry point to manage HTTP/HTTPS traffic into your cluster.


<img width="815" height="332" alt="Image" src="https://github.com/user-attachments/assets/9709538f-0d36-4d33-8d37-02fe36e2597b" />


Think of it as a reverse proxy + router that sits in front of your apps.

# Why use Ingress?

Without Ingress, people were used to LoadBalancing service like: Nginx, F5, Traefic. etc These are enterprise LB. 
They offer good LoadBalancing capability Like: 
Ratio based, TLS/SSL, path, domain, white listing,  black listing and more.

The Problem is Service only have simple LoadBalancing capabilty called as ``Round Robin`` If you have 10 requset Round Robin will send to pod no 1 and send to no 2 

One LoadBalancer per service (expensive). if you create 100 service using static IP address or Public IP addres. Cloud Provider will charge for each IP address.

With Ingress, you get:

- A single LoadBalancer / public entrypoint.
- Path-based and host-based routing.
- TLS/SSL termination (easy HTTPS).
- Integration with Ingress Controllers 

# How Ingress Works (Step by Step)
``Using Ingress controller`` In the Image named as (Ingress manage LoadBalancer), Then you create ``Ingress resource`` and Ingress controller will specify traffic of your Pods or Application.

Run this command to Install Ingress controller On Minikube (Local Cluster). 
```bash
minikube addons enable ingress
```

If you want to Ingress Controller for specific Ingress controller go to [documentation in Kubernetes](https://kubernetes.io/docs/concepts/services-networking/ingress-controllers/) Or you can directly search for On the Internet.


```bash
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: ingress # Change with your name
spec:
  rules:
  - host: "foo.bar.com" # This is your Domain web
    http:
      paths:
      - pathType: Prefix
        path: "/bar" # This is a Path In website
        backend:
          service:
            name: service1 # Change with your K8s Service
            port:
              number: 80
```

Before you use this Ingress by default It won't work properly because you haven't installed **Ingress Controller** so you can proceed Intallation Ingress Controller using this command:
## Minikube
```bash
minikube addons enable ingress
```

If you're using Minikube You have to Update **vim etc/host** and Provide the Ingress IP address with Domain example: 192.1.1.1 <<your-domain>>.

This Prerequisite for local cluster only. In production you don't need to change the file above you can proceed with Installation and look for the Ingress controller directly on browser.