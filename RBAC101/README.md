## Role-Based Access Control (RBAC) Overview

- Role-Based Access COntrol(RBAC) is a mechanism to ensure that the user has access to the defined system and to the desired level of his role.

- In Kubernetes, RBAC uses Rolebindings to bind the defined roles to the user for a particular namespace or Clusterbindings to bind the roles for the entire Kubernetes cluster.

Lets take a banking aplication, if there was no security measures at all then anybody who could access the application would just modify any other users data as well.
However in RBAC, there is no single user, who will have access to the entire system - not even the admins, reducing the risk of accidently bringing down the whole application. 
Instead, the roles (access levels) are defined based on their job requirements. The users would only have read access, while cashiers would have access to update the deposit amount.
similarly, managers would have access to reports and sysadmins would have permissions to modify/delete resources based on the job requirements.

## Creating a Kubernetes User Account Using X509 Client Certificate

Lets first create a kubernetes user(kube) using X509 certificate to understand RBAC in action.

First, we need to create a client key
```
openssl genrsa -out kube.key 2048
```

Next, we need a certificate signing request
```
openssl req -new -key kube.key -out kube.csr -subj "/CN=kube"
```

Now we have a user account with a key and signing request. Along with these details, we can now use the cluster key and certificate to sign in the request.
```
openssl x509 -req -in magalix.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out magalix.crt -days 300
```

## Note For Local And Self-Hosted Clusters Users
