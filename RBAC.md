# What is RBAC
RBAC stands for Role base access control. This is a method of regulating access to computer or network resources based on the roles of individual users within your organization.

RBAC is crucial for security and managing permissions in a multi user or team Kubernetes environment. Without it, every user or process would have the same level of access, which could lead to security breaches, accidental deletions, or unauthorized modifications.

If you have Kubernetes Cluster like, Minikube, KIND, etc Basically you will get Administrator users for This cluster
But when you Join in organization Administrator user has primary task to define Access to other team like QA, Dev etc to restrict undersirable access like deleting resources Inside the cluster It can be etcd or whatever.
RBAC will define depending about Role-based-Access-Controll so again RBAC simmilar to user management, You can manage access for your services or Application that are running on the cluster 

Enforce the principle of least privilege,Improve security,Enhance accountability,Streamline management