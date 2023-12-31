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

## Control Plane Components
Control Plane components runs only in the master node
1. API Server
2. etcd data-store
3. Scheduler
4. Controller Managers

  
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


Let's try to create nginx deployment with bitnami/nginx:latest docker image.
```
oc create deployment/nginx --image=bitnami/nginx:latest
oc get deployments/replicasets,pods
oc get all
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/4f78afc1-1832-45f7-a3db-243ec714ccb2)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/cdb6c3bf-4688-464c-95ad-fc49046d3057)

## Lab - Using plural form, singular form and short form Kubernetes/OpenShift commands
```
oc get pods
oc get pod
oc get po

oc get replicasets
oc get replicaset
oc get rs

oc get deployments
oc get deployment
oc get deploy
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/e2f4438c-f8ec-4b39-af58-cf2802782a39)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/4c8f6782-dbbe-4e93-a5ac-24ca4fc76578)

## Lab - Scale up nginx deployment
```
oc scale deploy/nginx --replicas=5
oc get po -w
oc get po
```
Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/6280c572-dd4f-4871-b2cb-80305f5a663c)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/48c71c38-dd42-429b-984b-6d124440bcb8)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/15564128-17e9-4fa9-b72d-a9b63aa33665)

## Lab - Scale down nginx deployment
```
oc scale deploy/nginx --replicas=3
oc get po -w
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/35f79456-16fb-41a6-846b-b05e28beef53)

## Lab - Pods are managed by ReplicaSetController
```
oc describe rs/nginx-5bccb79775
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/4cd910cc-20a3-43ad-af64-182cbab06802)

## Lab - Checking on which node the pods are running
```
oc get po -o wide
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/39597bf3-d084-4535-9353-0339e55243d2)

## Lab - Opening a terminal one a container running inside a Pod
```
oc rsh deploy/nginx
```

Expected ouput
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/5c3a1af7-77e6-49f3-81c9-137aa1ac8a17)

## Lab - Getting inside a particular Pod
```
oc get po
oc exec -it nginx-5bccb79775-hlvzg sh
ls
exit
kubectl exec -it nginx-5bccb79775-hlvzg sh
ls
cat index.html
exit
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/9de45df3-8a80-4326-935f-a218100b63d6)

## Lab - Choosing a specific container while opening a shell in Pod
```
oc exec -it -c nginx nginx-5bccb79775-hlvzg sh
ls
cat index.html
exit
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/d091e5ea-1978-470f-82a8-822020308cc4)

## Info - What happens in OpenShift cluster when we create a deployment
```
oc create deployment nginx --image=bitnami/nginx:latest --replicas=3
```

![deployment](deployment.png)


## Lab - Describe pod
```
oc describe po/nginx-5bccb79775-h42vx
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/2825718e-3f2e-4702-b82a-1278c2154e28)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/c5def75e-d4ca-4ef4-8126-6d10c78fa8ca)


## Lab - Port-forwarding a Pod to access the nginx web page ( Meant for Testing purpose )
```
oc port-forward pod/nginx-5bccb79775-8xnsv 8001:8080
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/459c955d-3b52-493e-85a3-569f6dc17023)

## Lab - Creating an internal ClusterIP service imperatively
```
oc get po -o wide
oc expose deploy/nginx --type=ClusterIP --port=8080
oc describe svc/nginx
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/73c563b4-d265-4077-86d3-b808eb5361fc)

## Lab - Creating an external nodeport service imperatively

For Node Port Services, Kubernetes/OpenShift has reserved ports between 30000 to 32767, whichever node in that range is available on all your openshift nodes that will be randomly picked by OpenShift and assigned for your Node Port service.

```
oc delete svc/nginx
oc get services

oc expose deploy/nginx --type=NodePort --port=8080
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/6d149a2c-a777-44a9-8fc3-ae344f6d9b61)

Accessing the external NodePort service
```
oc get nodes -o wide
curl http://192.168.122.62:30079
curl http://master-2.ocp.tektutor-ocp-labs:30079
curl http://worker-2.ocp.tektutor-ocp-labs:30079
```
In the above command, 30079 is the Node Port opened by OpenShift on every node in the openshift cluster.
The 192.168.122.62 is the IP address of the master-1 node, we could access the node port service from any node in the cluster.

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/4f4aa593-7d41-4c7e-81a4-6eb352ce1ede)


## Lab - Installing MetalLB Operator in Red Hat OpenShift Cluster
https://medium.com/tektutor/using-metallb-loadbalancer-with-bare-metal-openshift-onprem-4230944bfa35

Login to your Red Hat OpenShift webconsole from CentOS web browser
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/52e7d25f-91bc-45d5-8396-c6d1fdfe961b)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/13263ce3-d83b-4f09-9e8b-276c0032443c)

## Lab - Creating LoadBalancer external service
```
oc delete svc/nginx
oc get deploy
oc expose deploy/nginx --type=LoadBalancer --port=8080
oc get svc
oc describe svc/nginx
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/9b1fce8c-ac9f-4605-a1de-63fb518367a4)

Accessing the nginx web page
```
curl http://<nginx-service-external-ip>:8080

curl http://192.168.122.50:8080
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/ab6d55ce-8318-4be9-8be5-17be8fd55af2)
