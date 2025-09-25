# What is costume resources
Custom resources are a way to extend the functionality of a Kubernetes cluster beyond its built-in resources like Deployments and Services. They allow you to define and manage new kinds of objects within the Kubernetes API. Costume Resource are part of a larger system that includes Custom Resource Definitions (CRD) and custom controllers. Together, these components allow you to create powerful automation and management tools for your applications.

Let’s say you have a Kubernetes cluster. By default, the cluster provides several built-in resources such as Deployments, Services, ConfigMaps, and Secrets. These are native Kubernetes resources that work out of the box. However, if you want to go beyond these built-in resources, Kubernetes also allows you to extend its functionality by defining new custom resources. 

For example, if you want to extend the capability or extend API and implement GitOps in your Kubernetes cluster, you need CRD, CR, Costume Controller.
 
These are 3 resource to extend capability (API)

1. CR (Costume Resource)
A **Custom Resource** in Kubernetes is a way to extend the Kubernetes API by defining your own resource types. While Kubernetes comes with built-in resources like Pods, Deployments, and Services, custom resources let you add new types that fit your specific needs.

2. CRD (Custom Resource Definition)
CRD is a method to Defining a new type API to Kubernetes. to define it you have to submit CRD to Kubernetes which package in Yaml file. so CRD is a Yaml file which is used to introduce a new type of API to Kubernetes that will have all the fields a user can submit in the costume resource to Kubernetes so that The Kubernetes cluster will extended. 
While CR to spesify the yaml for ``new costume`` in Kubernetes and CRD will validate and extend the Custom resource that you've define.

3. Custom Controller
A Custom Controller is a program (usually written in Go, sometimes with frameworks like Kubebuilder or Operator SDK) that watches your custom resources and takes action when they change.
Without a controller, creating a custom resource doesn’t do anything—Kubernetes won’t "act" on your resource.
Controllers can be deployed via Helm chart, manifest, or Operator pattern.

In ingress if you are just deploy Ingress resource without controller nothing will happen and no use, simmilarly If you are just deploy costume resource is beeing watched by no one if nobody watching it nobody will happen. so the next step Devops should deploy Custom Controller either they can deploy using helm chart, manifest, or operator. then you can create this accross the custom controller or for the specific namespace. Now custome resource is verified by the controller and controller will perform the require action.


Custom Resource Definitions (CRDs), Custom Resources (CRs), and Custom Controllers work together to extend the Kubernetes API and automate the management of custom applications or infrastructure. The CRD defines the resource type, The CR is an instance of that type, The Controller watches for changes to CRs and reconciles them (performs actions, creates other resources, etc.)


# Extend Kubernetes with Istio
## Prerequisites
1. Check the Platform-Specific.[Prerequisites](https://istio.io/latest/docs/ambient/install/platform-prerequisites/)

2. Install the Helm client, version 3.6 or above.

3. Configure the Helm repository:
```bash
$ helm repo add istio https://istio-release.storage.googleapis.com/charts
$ helm repo update
```

## Base components
The base chart contains the basic CRDs and cluster roles required to set up Istio. This should be installed prior to any other Istio component.

```bash  
$ helm install istio-base istio/base -n istio-system --create-namespace --wait
```

## istiod control plane
The istiod chart installs a revision of Istiod. Istiod is the control plane component that manages and configures the proxies to route traffic within the mesh.

```bash 
$ helm install istiod istio/istiod --namespace istio-system --set profile=ambient --wait
```

## CNI node agent
The cni chart installs the Istio CNI node agent. It is responsible for detecting the pods that belong to the ambient mesh, and configuring the traffic redirection between pods and the ztunnel node proxy (which will be installed later).

```bash 
$ helm install istio-cni istio/cni -n istio-system --set profile=ambient --wait
```

## ztunnel DaemonSet
The ztunnel chart installs the ztunnel DaemonSet, which is the node proxy component of Istio’s ambient mode.

```bash 
$ helm install ztunnel istio/ztunnel -n istio-system --wait
```

## Verify the installation 
```bash
$ helm ls -n istio-system
```