# What is ConfigMap
A ConfigMap is a Kubernetes API object used to store non-confidential configuration data as key-value pairs. It's designed to decouple configuration from your application's container image, making your applications more portable and easier to manage. This allows you to update configuration settings without having to rebuild and redeploy your container.

Imagine you have application let's called it as backend application this app talk to Database, It retrieve some Information from Database and send It backs to the users when users has requested. The users request information such as: Database Port, Database Username, Database Password and Few more information that this application requires from the Database. This Information can be retrieve by ``Environment variables`` inside the application, 

Because the application shouldn't have these details (Database Port, Databes Username, Database Password) Hardcoded. If you have these details Hardcoded you will have a problem these details will change then user will get wrong Information.

To solve this problem you will not write this Information inside your application, Instead you can save the Information as part of your ``Enviroment variable`` or you will try to save this In a specifiv Path inside your application or file system and you will try to retrive this Information from the file system using ``OS Module``.

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
        env:                  # Here you atach env to configure with CM
         - name: DB-PORT      # This should be the name of Environment variable
           value From:
            configMapKeyRef:
             name: test-cm    # you should attach your CM which contain the DB-port
             key: db-port     # Fill with the data name in The CM
        ports:
        - containerPort: 80
```

4. To test this CM, first login to your Pods 
```bash
exec it <name of the Pods> -- /bin/bash

# Inside the pods check the value 

env | grep DB-PORT
``` 

