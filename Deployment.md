# What is Deployment

A **Deployment** in Kubernetes is a resource object that provides declarative updates for Pods and ReplicaSets. It allows you to define the desired state for your application (such as which container image to use, how many replicas to run, and update strategies), and Kubernetes will automatically manage the process of creating, updating, and scaling your application to match that state.

## Key features of Deployments:
- Automated Rollouts and Rollbacks: Easily update your application to a new version or revert to a previous one if something goes wrong.
- Auto Scaling: Increase or decrease the number of pod replicas as needed.
- Self-healing: Automatically replaces failed or unhealthy pods to maintain the desired state.

Deployments make it easy to manage the lifecycle of your applications in a Kubernetes cluster.

So if you want to do Zero downtime deployment or you want to bring Auto Scalling and Auto Healing. Don't deploy your app using Pod Instead deploy as deployment. 

## Deploy with Deployment

If you deploy a Deployment, It will create **Deployment** resource then It will create Intermediate resource called as **Replica set** and then replica set will create a **Pod**

### The end process using Deployment

You will create Deployment, this Deployment will roll out **Replica set**, And this will create Number of Pods that you have mention in the Deployment Yaml manifest. 

Replica set will ensure to implement Auto Healing capability, If user define 3 Pod in Yaml manifest, Replica set ensure that there are 3 Pod and Keep an eye on the manifest with Auto Healing.

