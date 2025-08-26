# What is Ingress

Ingress in Kubernetes is like a traffic controller for your cluster. Instead of use service stand alone to exposing each service with a NodePort or LoadBalancer, Ingress provides a central entry point to manage HTTP/HTTPS traffic into your cluster.


<img width="1135" height="1100" alt="Image" src="https://github.com/user-attachments/assets/a0e48d38-858e-4f04-b4cd-bde882a7da1d" />


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
## Prerequisite

Ingress Controller

A pod (NGINX, F5, Traefik, HAProxy, or cloud-specific) that watches for Ingress resources.

It’s the actual “traffic cop” that handles routing.

Ingress Resource

A Kubernetes object (apiVersion: networking.k8s.io/v1) that defines rules:

Which host → which service

Which path → which service

Flow Example

User opens browser → https://myapp.com/api

Request hits LoadBalancer → goes to Ingress Controller Pod

Controller checks Ingress rules → routes to the correct Kubernetes Service

Service forwards to backend Pods