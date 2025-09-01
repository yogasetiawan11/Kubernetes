# Architechture in Kubernetes

<img width="943" height="568" alt="Image" src="https://github.com/user-attachments/assets/c114997a-bcb2-4eba-b551-8f60f5f2c1f4" />

# Kubernetes Architecture

Kubernetes architecture is based on a **master-worker** (control plane and nodes) model. Here’s a breakdown of the main components:

## 1. Control Plane (Master)

The control plane manages the cluster and makes global decisions about the cluster (like scheduling). Key components include:

- **API Server**: The front end for the Kubernetes control plane. All communication (from users, CLI, or other components) goes through the API server.
- **etcd**: is a key-value store that holds all cluster data and configuration. It acts as a backup for your cluster information.
- **Controller Manager**: Watches the cluster state and makes changes to move the current state toward the desired state (e.g., starting pods, handling node failures).
- **Scheduler**: Responsible for scheduling your pod or resources on Kubernetes It can Assigns newly created pods to nodes based on resource requirements and policies.
- **Cloud Controller Manager**: It acts as a bridge between your Kubernetes cluster and the underlying cloud provider's infrastructure, You can use this If you're using Kubernetes in the cloud provider (eks, aks),

## 2. Nodes (Workers)

Nodes are the machines (VMs or physical servers) that run your application workloads. Each node contains:

- **Kubelet**: Like a command line interface (CLI) that ensures containers are running in a pod as expected.
- **Kube Proxy**: Handles network routing and load balancing for services on the node.
- **Container Runtime**: Software responsible for running containers (e.g., Docker, containerd).

Workers Node responsible for running your application These are 3 components you have technically everything to run your application.

## 3. Pods

- The smallest deployable unit in Kubernetes. A pod can contain one or more containers that share storage, network, and a specification for how to run the containers.

## How It Works

1. **User submits a deployment** (via kubectl or API).
2. **API Server** receives the request and stores the configuration in **etcd**.
3. **Scheduler** assigns the pod to a suitable node.
4. **Kubelet** on the node pulls the container image and starts the pod.
5. **Kube Proxy** manages networking so the pod can communicate with others and receive traffic.

This architecture allows Kubernetes to automate deployment, scaling, and management of containerized application efficiently.
