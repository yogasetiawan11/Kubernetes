# What is RBAC
RBAC stands for Role base access control. This is a method of regulating access to computer or network resources based on the roles of individual users within your organization.

RBAC is crucial for security and managing permissions in a multi user or team Kubernetes environment. Without it, every user or process would have the same level of access, which could lead to security breaches, accidental deletions, or unauthorized modifications.

If you have Kubernetes Cluster like, Minikube, KIND, etc Basically you will get Administrator users for This cluster
But when you Join in organization Administrator user has primary task to define Access to other team like QA, Dev etc, to restrict undersirable access like deleting resources Inside the cluster It can be etcd or whatever.
RBAC will define depending about Role-based-Access-Controll so again RBAC simmilar to user management, You can manage access for your services or Application that are running on the cluster 

These are 3 Primary component of RBAC
1. Service account/users
2. Roles/Cluster Role
3. Role Binding/ Cluster Role Binding

1. Service Accounts vs. Users

Users: A user is a human and is typically managed externally. Kubernetes doesn't have a User object like Linux. It's not something you create or describe with ``kubectl``. a user's identity is provided by an external source like an enterprise directory (e.g., LDAP, Active Directory), a certificate, or a third-party identity provider (like Keycloak or Google). Their permissions are assigned to their username or group membership.

Service Accounts: A service account is an identity for non human processes or workloads, like an application running in a pod or an external CI/CD tool. Service accounts are namespaced objects you can create and manage directly within Kubernetes. By default, every pod gets a service account, which it uses to authenticate with the component Kubernetes API.


2. Roles vs ClusterRoles

Role: A Role is a namespaced object that defines permissions within a single namespace. For example, a Role in the dev namespace could grant the ability to "get" and "list" pods, but only within that specific dev namespace.

ClusterRole: A ClusterRole is a non-namespaced object that defines permissions across the entire cluster. It can grant permissions to cluster-scoped resources (like nodes), or to namespaced resources across all namespaces (like viewing all pods in the cluster). ClusterRoles are also used to define common permission sets that can be reused by a RoleBinding in multiple namespaces.

3. RoleBinding vs. ClusterRoleBinding

RoleBinding: A RoleBinding links a user, group, or service account to a Role within a specific namespace. It grants the permissions defined in the Role to the subject, but only within that namespace. A RoleBinding can also be used to bind a ClusterRole to a subject, but the permissions will still only apply within the RoleBinding's namespace.

ClusterRoleBinding: A ClusterRoleBinding links a user, group, or service account to a ClusterRole across the entire cluster. This is how you grant cluster-wide administrative permissions to a subject. For example, to give a user permission to view all nodes, you would use a ClusterRoleBinding that references a ClusterRole with get permissions on nodes.

