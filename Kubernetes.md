## Introduction
Orchestration Platform to manage containers

## K8s Components

### Node
Virtual or Physical or Cloud machine

### Pod
Smallest unit. Containers are deployed and run in pod. A pod runs the image/app and sometimes it can contain multiple apps as side-car. Layer of abstraction over container.

### Service & Ingress
Service have physical ip address created for service to service communication and are independent ie if app-service(pod) goes down. Another replicate will spin up and attach to the service.
Service also acts as an load balancer. Service is for internal ip and ingress is for external ip so that outside network can access it

### ConfigMap & Secrets
External configurations and properties like db username/password are stored in ConfigMap and Secrets. Private info like passwords are stored in Secrets as base64 encoded.

### Volumes
Volumes are used to map persistent stateful objects like file/database. They can be mapped to cloud storage or physical storage

### Deployment & StatefulSet
Blueprint of pod which contain the replica info as such. We don't directly work with pod and instead use deployment. Abstraction over pods.
StatefulSet is for stateful application like database as it's replication using deployment will cause data inconsistencies. 

DB are often hosted outside of K8s cluster.

## K8s Architecture

### K8s comprises of Master and Worker Nodes. Worker node is responsible for hosting the pods and Master node is responsible for managing the worker nodes

## Components of Worker Nodes

### Docker Engine
The docker engine process is responsible for running the container ie Container Runtime. K8 supports any valid container runtime

### Kubelet
The kubelet process manages the start/stop of pods and is responsible for interacting with the pods and the cpu and other processes of the node

### Kubeproxy
kube-proxy maintains network rules on nodes. These network rules allow network communication to your Pods from network sessions inside or outside of your cluster.
kube-proxy is a network proxy that runs on each node in your cluster, implementing part of the Kubernetes Service concept.

## Components of Master Nodes

### Api-Server
The api-server acts as the gateway for communicating with the cluster. It provides a front end to monitor and manage the cluster

### Scheduler
Scheduler schedules the start of pods

### Controller Manager
Controller manager decides in which node the new deployment pod should be start/stops and notifies scheduler to schedule it

### etcd
Key-Value data store which contains the cluster information. All other k8s components store the state information about the cluster in the etcd data store. In case of replica of master nodes, the etcd data store is distributed.

## K8s Commands
Create minikube cluster
```minikube start --vm-driver=hyperkit
kubectl get nodes
minikube status
kubectl version
```

Delete cluster and restart in debug mode
```minikube delete
minikube start --vm-driver=hyperkit --v=7 --alsologtostderr
minikube status
```

Kubectl commands
```
kubectl get nodes
kubectl get pod
kubectl get services
kubectl create deployment nginx-depl --image=nginx
kubectl get deployment
kubectl get replicaset
kubectl edit deployment nginx-depl
```
Debugging
```
kubectl logs {pod-name}
kubectl exec -it {pod-name} -- bin/bash

kubectl delete deployment nginx-depl
```

Deployment Config file
```
vim nginx-deployment.yaml
kubectl apply -f nginx-deployment.yaml
```

delete with config
```
kubectl delete -f nginx-deployment.yaml
```
#Metrics
```
kubectl top
```

kubectl cp --help
Copy files and directories to and from containers.
Examples:
!!!Important Note!!!
Requires that the 'tar' binary is present in your container image.  If 'tar' is not present, 'kubectl cp' will fail.

Copy /tmp/foo_dir local directory to /tmp/bar_dir in a remote pod in the default namespace
```
kubectl cp /tmp/foo_dir <some-pod>:/tmp/bar_dir
```

Copy /tmp/foo local file to /tmp/bar in a remote pod in a specific container
```
kubectl cp /tmp/foo <some-pod>:/tmp/bar -c <specific-container>
```

Copy /tmp/foo local file to /tmp/bar in a remote pod in namespace <some-namespace>
```
kubectl cp /tmp/foo <some-namespace>/<some-pod>:/tmp/bar
```

Copy /tmp/foo from a remote pod to /tmp/bar locally
```
kubectl cp <some-namespace>/<some-pod>:/tmp/foo /tmp/bar
```
```
-c, --container='': Container name. If omitted, the first container in the pod will be chosen
```

```Usage:
kubectl cp <file-spec-src> <file-spec-dest> [options]
```

Use "kubectl options" for a list of global command-line options (applies to all commands).

cp apiwiz-system/sso-tonik-api-78d99b96b8-ct5cm:/logs/itorix-sso-error.log /var/lib/jenkins/sso-log-backup/itorix-sso-error.log