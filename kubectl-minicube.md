# Install Minikube
minikube is local Kubernetes, focusing on making it easy to learn and develop for Kubernetes.

## Install minikube (Linux x86-64)

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb
```

# Install Kubectl
Before you Begin You must use a kubectl version that is within one minor version difference of your cluster. For example, a v1.33 client can communicate with v1.32, v1.33, and v1.34 control planes. Using the latest compatible version of kubectl helps avoid unforeseen issues.

this method only works for Linux X86-64 If you have different version chek Kubernetes Documentation.

1. First Install The latest release with the command:

```bash
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
```

To install specific version make a change of the command Like:

```bash
curl -LO https://dl.k8s.io/release/v1.33.0/bin/linux/amd64/kubectl
```

This command will install version ``v1.33.0`` you can modify this session depends on your needs.

2. Install ``kubectl``
```bash
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
```

3. Verify the installation has done.
```bash
kubectl version --client
```

Or use this for detailed view of version:

```bash
kubectl version --client --output=yaml
```