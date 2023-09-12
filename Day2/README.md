# Day 2

## What is Container Orchestration Platform ?
- Container Orchestration Platform offers the below features
  1. Provides an eco-system or environment to make your application Highly Available (HA)
  2. It has in-built monitoring features
     - It frequently monitors the health of the containerized application workloads
     - It frequently monitors the liveliness of the containerized application workloads
     - When it detects the application is non-responsible or down, then it would replace with another containerized application
  3. It supports scale up/down based on user-traffic to your application workloads
  4. Rolling update
     - We can upgrade the application from one version to other without any downtime
     - We can also rollback when the newly upgraded version of your application is found to be faulty
  5. Supports in-built Load Balancing to containerized application workloads
  6. Also supports using external load balancers in private,public and Hybrid cloud environments
  7. Supports exposing the application workloads internally or externally
  8. We can run one time activities like backup as Jobs
  9. We can schedule repeatitive Jobs every day, week a particular time CronJobs
- Cluster of Servers work together to form the Orchestration Platform
- Two Types of Servers
  1. Master
  2. Worker
- In a production grade setup, many Master Nodes will be available to ensure if one Master stops responding other Master would be available to keep the cluster responsive
- Self-healing Platform
  - It not only observes instability in user application workloads, it is also capable of repairing it own components when they are non-responsive or when they crash
ExamplesL:-
1. Docker SWARM
2. Kubernetes
3. Red Hat OpenShift

## Docker SWARM
- it is native orchestration platform developed and maintained by Docker Inc organization
- it only supports managing docker containers
- it is not production-grade, hence not many companies use them in production, they good for learning and prototypes or testing in Dev/QA environment
- it is very light-weight, hence can be easily installed even on a normal laptop
- It is free for personal and commercial use

## Kubernetes
- it is developed by Google and donated to open source community
- it is still backed by Google along with many opensource contributers
- it is developed in Go lang
- it is opensource
- supports command-line
- there is a web based Dashboard, but it is not production grade, it is not secure
- it is time tested, as Google internally used this several years before they made it opensource
- it is production grade
- it is used by many companies of different size in different domanin
- highly stable, capable of handling complex, resource hungry applications without any hassle
- It also supports extending Kubernetes features via Custom Resource Definitions (CRD)
- The container orchestration features are implemented as REST API
- Kubernetes supports many different Container Engines and Container Runtimes that supports/implemented Container Runtime Interface(CRI)
- Installing Kubernetes is relatively easier compared to Red Hat OpenShift
- The below are some of the Kubernetes Resources
  - Deployment
  - ReplicaSet
  - Pod
  - Job
  - CronJob
  - DaemonSet
  - StatefulSet
  - Services
    1. ClusterIP
    2. NodePort
    3. Load Balancer
  - Custom Resource Definition (CRD)
  - Ingress 

## Red Hat OpenShift
- Red Hat OpenShift is developed on top of Google Kubernetes
- it is an enterprise product, we need buy license to use it
- Red Hat(an IBM company) provides world-wide support
- supports all features of Kubernetes
- also supports many additional features
- the additional features are added by using Kubernetes Custom Resource Definition (CRD)
- it is production grade
- it supports both command-line and Web Console
- it supports User Management
- We could even deploy Jenkins within OpenShift
- Older version of OpenShift upto 3.x supported Docker, starting from OpenShift 4.x support for Docker was removed
- Only supports Podman Container Engine with CRI-O Container Runtime
- Installing Red Hat OpenShift is very complex compared to Kubernetes
- Starting from OpenShift 4.x, the operating sytem supported with Nodes/Servers is limited to Red Hat Enterprise Linux or Red Hat Enterprise Core OS
- Master Nodes only supports Red Hat Enterpise Core OS
- Worker Nodes have two choices
  - They could either use Red Hat Enterprise Linux (RHEL) or Red Hat Enterpise Core OS (RHCOS)

## CI/CD with Kubernetes/OpenShift
- TekTon is a cloud-native server-less CI/CD framework that works within Kubernetes/OpenShift
- It is a competing product, which is alternate for Jenkins/Cloudbees/Team City/Bamboo/TFS, etc.,

## Kubernetes/OpenShift tools
- kubelet - Container Agent that interacts with CRI-O Runtime via Container Runtime Interface (CRI)
- kubeadm - administrative tool used to bootstrap master node and add/remove worker nodes to the OpenShift cluster
- kubectl - kubernetes client tool also supported in OpenShift
- oc - OpenShift native client tool used by users to interact with OpenShift cluster to manage containerized application workloads

## What is a Kubernetes/OpenShift Controller?
- Controller is a application that monitors a particular type of Kubernetes/OpenShift resource
- Controller ensures the desired count and actual count of resources are same
- Controller are the one which provides monitoring functionality to the Container Orchestration Platform
- To manage every of Kuberenetes/Openshift resource there is one type of Controller
- Examples
  - Deployment Controller
  - ReplicaSet Controller
  - Endpoint Controller
  - DaemonSet Controller
  - StatefulSet Controller
  - Job Controller 

## What is Deployment Controller?
- Deployment Controller receives notification events anytime something changes in Deployment
  - New Deployment Created
  - Deployment updated
  - Deployment deleted
- Deployment Controller is the one that manages ReplicaSet resource

## What is ReplicatSet Controller?
- ReplicaSet Controller receives notification events anytime something changes in ReplicaSet
  - New ReplicaSet created
  - ReplicaSet updated
  - ReplicaSet deleted
- ReplicaSet Controller is the one that manages the Pod resources
  
## What is a Deployment?
- when application are deployed in Kubernetes/OpenShift, they are deployed as Deployment
- Deployment resources contains the following
  - name of the deployment
  - the container image that must be used in the Pod
  - the number of Pod instances that be created as part of the deployment

## What is a ReplicaSet?
- is a configuration that tells how many Pod instances are supposed to be running as part of a appliction deployment
- There would be
  - desired count of Pods
  - actual count of Pods
  - ReplicaSet is referred by ReplicaSet controller to create/manage the number of Pod instances
  - The ReplicaSet controller keeps monitors the number of actual Pods vs desider count of Pods, whenever it sees a different it acts to match the number of desired pod count with the actual count

## What is a Pod?
- Pod is a group of related containers
- one Pod represents one application
- In Kubernetes/OpenShift IP address is assigned on the Pod level
- Containers in the same Pod shares the IP address
- As per Best practices, only one main application should part of one Pod
- One Container per Pod is the recommended practice
- User application will be running in one of the container within Pod
- Pods are managed by ReplicaSet Controller
- the smallest unit that can be deployed and managed is Pod
- generally deploying a Pod directly is not the recommended practice but is possible


  
# OpenShift commands

## Lab - Creating a Pod in Docker
```
docker pull google/pause:latest
docker images
docker run -d --name nginx_pause --hostname nginx google/pause:latest
docker ps
docker inspect nginx_pause | grep IPA

docker run -d --name nginx --network=container:nginx_pause nginx:latest
docker ps
docker exec -it nginx sh
hostname
hostname -i
exit
```

### Things to notice 
- the IP address of nginx_pause container and nginx containers are same.
- though we never assigned a hostname for the nginx container, the nginx container's hostname is nginx
- the reason is, nginx container shares the network of nginx_pause container
- every Pod created in Kubernetes/OpenShift, there is always a secret infra-container called pause container apart from the main application container

### Best Practices
- though technically only one Pod many have many main application containers, ideally we should only create one main application per Pod
- the reason is, if we allow many main application containers per Pod, we won't be able to scale up/down one main applciation container independent of other main application containers
- ideally we should create two separate pod for two main applications, so that we scale them independent of each other

## Lab - Listing OpenShift nodes
```
oc get nodes
oc get nodes -o wide
oc version
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/d2b210c3-6450-4573-9b38-4a0e3642eed9)

## Lab - Finding more details about a node
```
oc get nodes
oc describe node master-1.ocp.tektutor-ocp-labs
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/ddb8abf4-a4d3-41c2-b316-0ba26036afb8)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/e15755c1-6e29-47f5-85c0-e0cb47cd4cee)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/0ff48c99-aec5-4062-a6f0-2aa78b68ab53)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/2b39dca1-659e-46bd-8b8b-005c22c51fe3)

## Lab - Finding IP address of node, OS installed in node, Container Runtime installed in node with wide mode
```
oc get nodes -o wide
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/d2b54323-46b2-4776-b8d7-7de1db3e4c0b)

## Lab - Editing node details
```
oc edit node/master-1.ocp.tektutor-ocp-labs
```

Expected output

![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/3e7693dd-e4fa-4333-a578-5f00505b7e1a)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/bfc95351-4d11-44a5-9310-945a2affa3d2)

## Lab - Creating a project in OpenShift
You need to replace 'jegan' with your name to avoid conflicts the lab machine.
```
oc new-project jegan
oc project
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/6d72d455-5104-4207-832c-b947ec25ac34)


## Lab - Creating our first application deployment into OpenShift
```
oc project jegan
oc create deployment nginx --image=nginx:latest
oc get deployments
oc get repilcasets
oc get pods
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/16aedb29-1e39-4b20-8c81-699036f1105a)

You would observed that the Pod keeps crashing and OpenShift is keep attempting to repair it by restarting the Pod.

Let us check pod log to understand the root cause of the Pod crash
```
oc get pods
oc logs nginx-654975c8cd-rjxlg
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/7581317f-e104-4bdd-9aff-b02275958468)

From the above log, we can understand that the container image nginx:latest from Docker Hub portal is attempting to create a folder under /var folder, which isn't allowed in Red Hat Enterprise Core OS. Hence, we can't use the nginx:latest container image to deploy nginx in OpenShift, but the same image will work perfectly in Kubernetes Orchestration Platform.

Let's delete the nginx deployment
```
oc delete deploy/nginx
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/c0d4150d-b152-43d8-9a46-3bb37cee7bb4)
