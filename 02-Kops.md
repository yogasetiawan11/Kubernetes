# What is KOPS
Kubernetes operation is an open source platform that manage cluster on Different Kubernetes Platform like (EKS, AKS, GKE, OenShift) Think of it like as a ``Kubectl`` for your clusters 

``KOPS`` will help you to create, destroy, upgrade, and mantain production grade, Highly available, In Kubernetes Cluster. 

It can also help to provision the necessary Cloud Infrastructure. (AWS, GCE, Azure)

## Kubernetes Installation using KOPS

Dependencies Required:
1. Python
2. AWS cli
3. kubectl

## Install KOPS (Linux) 
```bash
curl -LO https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64

chmod +x kops-linux-amd64

sudo mv kops-linux-amd64 /usr/local/bin/kops
```

## Provide the permission below to your IAM user
- Amazon IAM Full access
- Amazon VPC Full access
- Amazon S3 Full access
- Amazon EC2 Full access

## Proceed to configure AWS cli on EC2 or Laptop
```bash
aws configure
```
# Kubernetes cluster installation
Follow this steps and adjust each the command carefully

## Create S3 to store KOPS object.
```bash
aws s3api create-bucket --bucket kops-yogasn-storage --region us-east-1
```
You need to create S3 because KOPS basically manage your hundered of Kubernetes cluster. It can be easy to manage the cluster using S3.

## Create the cluster
```bash
kops create cluster --name=demok8scluster.k8s.local --state=s3://kops-yogasn-storage --zones=us-east-1 --node-count=1 --node-size=t2.micro --master-size=t2.micro  --master-volume-size=8 --node-volume-size=8
```
You can modify according to your wish. 
Explanation:
``--name=`` this show your KOPS name
``--s3=`` address of the s3 that you've made
``k8s.local`` This is the ```local domain`` But in production or in organization you have to use Domain of your company and configure using Route 53

rest all the thing in the command is still same 


## In the final steps
you have to activate EKS and this is not free 

```bash
kops edit cluster myfirstcluster.k8s.local
```

Build the cluster

```bash
kops update demok8scluster.k8s.local --yes
```

Run the command Bellow to verify the cluster installation.
```bash
kops validate cluster demok8scluster.k8s.local
```