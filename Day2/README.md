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
- supports all features of Kubernetes
- also supports many additional features
- the additional features are added by using Kubernetes Custom Resource Definition (CRD)
- it is production grade
- it supports both command-line and Web Console
- it supports User Management
  

# OpenShift commands

## Lab - Listing OpenShift nodes
```
oc get nodes
oc get nodes -o wide
oc version
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/d2b210c3-6450-4573-9b38-4a0e3642eed9)
