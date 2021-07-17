## Introduction
Orchestration Platform to manage containers

## K8s Components

### Node
Virutal/Phyical/Cloud machine

### Pod
Smallest unit. Containers are deployed and run in pod. A pod runs the image/app and sometimes it can contain multiple apps as side-car. Layer of abstraction over container.

### Service & Ingress
Service have physica ip address created for service to service communication and are independent ie if app-service(pod) goes down. Another replicate will spin up and attach to the service.
Service also acts as an load balancer. Service is for internal ip and ingress is for external ip so that outside network can access it

### ConfigMap & Secrets
External configurations and properties like db username/password are stored in ConfigMap and Secrets. Private info like passwords are stored in Secrets as base64 encoded.

### Volumes
Volumes are used to map persistent stateful objects like file/databas. They can be mapped to cloud storage or physical storage

### Deployment & StatefulSet
Blueprint of pod which contain the replica info as such. We don't directly work with pod and instead use deployment. Abstraction over pods.
Statefulset is for stateful application like database as it's replication using deployment will cause data inconsistencies. 

DB are often hosted outside of K8s cluster.