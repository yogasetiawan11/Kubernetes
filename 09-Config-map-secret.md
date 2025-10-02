# What is ConfigMap
A ConfigMap is a Kubernetes API object used to store non-confidential configuration data as key-value pairs. It's designed to decouple configuration from your application's container image, making your applications more portable and easier to manage. This allows you to update configuration settings without having to rebuild and redeploy your container.

Imagine you have application let's called it as backend application this app talk to Database, It retrieve some Information from Database and send It backs to the users when users has requested. The users request information such as: Database Port, Database Username, Database Password and Few more information that this application requires from the Database. This Information can be retrieve by ``Environment variables`` inside the application, 

Because the application shouldn't have these details (Database Port, Databes Username, Database Password) Hardcoded. If you have these details Hardcoded you will have a problem these details will change then user will get wrong Information.

To solve this problem you will not write this Information inside your application, Instead you can save the Information as part of your ``Enviroment variable`` or you will try to save this In a specific Path inside your application or file system and you will try to retrive this Information from the file system using ``OS Module``.

# The difference between Secret and ConfigMap
Both of them are used to store the Information. ``ConfigMap`` Stores data in plain text, while ``Secret`` Stores data in base64 encoded format .for example you might want to store json data, you want to save some key value pair inside Kubernetes Pods application You can use both of them for the same purpose. But ``Secret`` used for store sensitive Information whereas ``ConfigMap`` used for non-sensitive Information.

with Secret the data is encrypted at rest, with Secret you can enforce strong RBAC, so for the entire Secret resource In Kubernetes you can say only Devops Engineer should have access to The Secret or this technique also known as ``Least Privillage``. 

## Example Config Map
In this example I'll store my sql Data Base Inside Config Map
1. Create cm File ``vim cm.yml`` Inside this file in ``data`` you can fill it with The information like Database Port, etc.

```bash
apiVersion: v1
kind: ConfigMap
metadata:
  name: game-demo
data:
  db-port: "3306"
```

2. apply the file above
```bash
kubectl apply -f cm.yml
kubectl get cm                       # List all config map
kubectl describe cm <name-cm>        # To see the data inside Config Map
```

3. Afterwards you can configure this Information (Config Map) with your Deployment 

This is default example of Deployment in kubernetes 
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        env:                  # attach env to configure with CM
         - name: DB-PORT      # This should be the name of Environment variable
           valueFrom:
            configMapKeyRef:
             name: test-cm    # Fill it with file name of CM 
             key: db-port     # Fill it with the data name in The CM
        ports:
        - containerPort: 80
```

4. To test this CM, first login to your Pods 
```bash
kubectl exec -it <name of the Pods> -- /bin/bash

# Inside the pods check the value 

env | grep DB-PORT
``` 


## Generate token using service account in the namespace
[click this link](https://kubernetes.io/docs/reference/access-authn-authz/service-accounts-admin/#:~:text=To%20create%20a%20non%2Dexpiring,with%20that%20generated%20token%20data.)

This is how you use Config map inside application as environment variable.
But there is a problem with this way where you cannot change the Information or the value inside the enviroment variable because container doesn't allow changing the enviroment variable, you have to recreate the container.
In production you also cannot delete or recreate deployment It will incur traffic lost. To solve this problem Kubernetes comes with Volume Mounts

# Volume Mounts
In Kubernetes, a volume mount is the way you attach a storage volume into a container so that the container can read and write data. You can consider it as a file where you can ``Mount`` Information inside a file so developer can read the Information from a file instead of ``Environment Varible``.

## Example of Volume Mounts
```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        volumeMounts:             # Here you mount the volume
         -  name: db-connection   # Tell specific Volume you want to Mount
            mountPath: /opt       # specify a path u want to store the CM
        ports:
        - containerPort: 80
      Volumes:                    # Here creation of the volume
        - name: db-connection     # This is a name of the volume
          configMap:              # In this case I use type Config Map 
            name: test-cm         # the name of the Config Map

# In kubernetes you can create external, internal, persistent volume, config map volume. In this case I create Config map volume
```
## To test this CM, first login to your Pods 
```bash
kubectl exec -it <name of the Pods> -- /bin/bash

# Inside the pods check the value 

cat /opt/db-port
``` 

# What is a Secret in Kubernetes?
A Secret is a Kubernetes object used to store sensitive data such as:
- Passwords
- API keys
- TLS certificates
- Database connection strings

Instead of hardcoding these values in Pod specs or ConfigMaps, you keep them in a Secret and mount them into Pods (via env variables or volumeMounts).

## Create generic secret
```bash
kubectl create secret generic test-secret --from-literal=db-port="3360"

# kubectl create secret generic => tells Kubernetes you want to create a generic Secret.
# test-secret => name of the secret 
# key => db-port
# value => 3360
```


## Drawbacks / Limitations of Secrets
Even though they’re called “Secrets”, Kubernetes does not encrypt them by default.

1. Base64 encoding only
Secrets are just Base64 encoded → anyone with access can decode easily.
Example: ```echo YWRtaW4= | base64 --decode``` → admin.

2. Stored in etcd in plaintext
Kubernetes stores Secrets in etcd.
If etcd is not encrypted (default before v1.13), Secrets are stored unencrypted on disk. Whoever has access to etcd has access to Secrets.

3. RBAC misconfiguration risk
If you don’t restrict access properly, anyone with read access to Secrets can view sensitive data.

4. Exposure via logs / debugging
If Secrets are passed as env vars, they might show up in:

- kubectl describe pod
- Crash dumps
- Debug logs

5. Node compromise risk
Once a Pod mounts a Secret, any process in that container can read it.
If the node itself is compromised, an attacker can read Secrets mounted on Pods running there.

Instead you can use 3rd tools like Hashicorp Vault to store your secret securely