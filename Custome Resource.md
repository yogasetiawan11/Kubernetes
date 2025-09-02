# what is costume resources
Costume Resource are extention of The Kubernetes Cluster API

Let’s say you have a Kubernetes cluster. By default, the cluster provides several built-in resources such as Deployments, Services, ConfigMaps, and Secrets. These are native Kubernetes resources that work out of the box.

However, if you want to go beyond these built-in resources, Kubernetes also allows you to extend its functionality by defining new custom resources. 

For example, if you want to extend the capability or extend API and implement GitOps in your Kubernetes cluster, you need CRD, CR, Costume Controller.

First step user will 
There are 3 resource to extend capability (API)

1. CR (Costume Resource)
A **Custom Resource** in Kubernetes is a way to extend the Kubernetes API by defining your own resource types. While Kubernetes comes with built-in resources like Pods, Deployments, and Services, custom resources let you add new types that fit your specific needs.
2. CRD (Custom Resource Definition)
CRD is a method to Defining a new type API to Kubernetes. to define it you have to submit CRD to Kubernetes which package in Yaml file. so CRD is a Yaml file which is used to introduce a new type of API to Kubernetes that will have all the fields a user can submit in the costume resource to Kubernetes so that The Kubernetes cluster will extended. 
While CR to spesify the yaml for ``new costume`` in Kubernetes and CRD will validate and extend the Custom resource that you've define.

3. Custom Controller
In ingress if you are just deploy Ingress resource without controller nothing will happen and no use, simmilarly If you are just deploy costume resource is beeing watched by no one if nobody watching it nobody will happen. so the next step Devops should deploy Custom Controller either they can deploy using helm chart, manifest, or operator. then you can create this accross the custom controller or for the specific namespace. Now custome resource is verified by the controller and controller will perform the require action.

Custom Resource Definitions (CRDs), Custom Resources (CRs), and Custom Controllers work together to extend the Kubernetes API and automate the management of custom applications or infrastructure.

