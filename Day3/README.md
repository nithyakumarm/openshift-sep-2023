# Day 3

## Lab - Listing all projects in OpenShift cluster
```
oc get projects
oc get namespaces
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/d59505e5-d7d1-4869-b2f2-c7a297090d0c)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/0e27a513-81ad-492d-aa38-3277141c6246)

## Lab - Understanding Service Discovery in OpenShift/Kubernetes
- Service discovery is nothing but accessing a service by it name which gets resolved into its respective IP address
- DNS Server helps resolving the name of the service to the IP address of the service
- Service discovery works only within the OpenShift cluster and it is supported by every type of service
1. ClusterIP
2. NodePort and
3. LoadBalancer

Delete any existing service you have in your project
```
oc project
oc delete svc/nginx
oc expose deploy/nginx --type=ClusterIP --port=8080
oc get svc/nginx
oc describe svc/nginx
```

Now we need test pod with curl utility, hence let's create a pod as shown below
```
oc run hello --image=tektutor/spring-tektutor-helloms:latest
oc get po -w
oc exec -it hello sh
curl http://<cluster-ip-of-nginx-service>:8080
curl http://172.30.222.144:8080
```

Accessing a service by its name ( Service Discovery ), in the same hello pod shell you can try this
```
curl http://nginx:8080
```

How the Pod is able to resolve the nginx service name to its corresponding IP address is
```
cat /etc/resolv.conf
```
The above file uses the DNS nameserver 172.30.0.10, this entry is created in each Pod by kubelet at the time it cretes the Pod on the respective node where kubelet is running.

If you wish to see the DNS Nameserver, you can try this
```
oc get svc --all-namespaces | grep -i dns
oc describe svc/dns-default -n openshift-dns
```

## Lab - Getting help info about any OpenShift resource
```
oc explain deployment
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/c3c2987d-c97a-4a5c-a429-f90a827dbf1e)

## Lab - Declaratively creating nginx deployment

Let's delete the project and recreate project with the same name
```
oc delete project/jegan
oc new-project jegan
```

Let's generate the nginx deployment yaml file as shown below
```
oc create deployment nginx --image=bitnami/nginx:latest --dry-run=client -o yaml
oc create deployment nginx --image=bitnami/nginx:latest --dry-run=client -o yaml > nginx-deploy.yml
cat nginx-deploy.yml
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/00e300a2-6bb2-4073-beee-9e8f0c6d089b)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/b4a7bc1f-dc59-41c6-8f9d-0705f218b307)

## Lab - Creating a nginx deployment in declarative style using yaml(manifest) file
The oc create command should be used when the nginx deployment you are creating through file doesn't already exist in the OpenShift cluster.  In otherwords, only the first time we should use create command and subsequent times we should use apply command to update any changes on the existing resources using the same yaml file.

Interestingly, the apply command can be used everytime unlike the create command. 

```
cd ~/openshift-sep-2023
git pull
cd Day3/declarative-manifests
oc create -f nginx-deploy.yml --save-config
oc create -f nginx-deploy.yml --save-config
oc apply -f nginx-deploy.yml
oc get deploy,rs,po
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/b77f5eec-df2f-4542-a511-f92b9a5007c4)

## Lab - Scale up nginx deployment in declarative style

Update the nginx-deploy.yml, replicas value from 2 to 5 and save it before apply it.
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/62fe9839-0e21-4c57-a199-7029d241d172)

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/642f27b9-8470-4d9e-93e6-2d9343c0251d)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/c0a31f0e-6b67-4cf4-9d17-8e3b2bb5b725)

## Lab - Scale down nginx deployment in declarative style

Update the nginx-deploy.yml file replicase value from 5 to 2, save it before applying it.
```
cd ~/openshift-sep-2023
git pull
cd Day3/declarative-manifests
gedit nginx-deploy.yml
cat nginx-deploy.yml
oc apply -f nginx-deploy.yml
oc get po
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/216b40c2-ae8e-4260-a9b0-3b297ea5017f)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/4bb2805c-81dc-4c3d-9b7b-edf0137870d0)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/16a1dfdf-13fa-47c0-8bdb-8e5479433617)

## Lab - Using Rolling update to upgrade your live application from one version to other without downtime

First you can delete the nginx deploy that is already running in the cluster.
```
cd ~/openshift-sep-2023
git pull
cd Day3/declarative-manifests

oc delete -f nginx-deploy.yml
```

Then update the image in the nginx-deploy.yml from "bitnami/nginx:latest" to "bitnami/nginx:1.24" and save it.
You can now apply/create this change into the cluster
```
cd ~/openshift-sep-2023
git pull
cd Day3/declarative-manifests
cat nginx-deploy.yml
oc get deploy
oc apply -f nginx-deploy.yml
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/6690b72a-401b-4ece-8421-dc2e633189af)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/6c66ef09-81cc-4cd1-b7bb-a857380fcf31)

Checking the rolling update status
```
oc rollout status deploy/nginx
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/1c56fb04-46ef-42d6-9ec1-1037bc8f2afb)


Checking the rolling update revision history
```
oc rollout history deploy/nginx
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/829a0f9e-a29d-4277-b4a9-8069479c5b92)

Let's update the image version from 1.24 to 1.25
```
cat nginx-deploy.yml
oc apply -f nginx-deploy.yml
oc get po -w
oc get po
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/d2c2c6ea-7360-4e5f-8062-e9ece0d0b594)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/0152e29c-9265-4e30-9b3c-f75d37874e42)

Rolling back from 1.25 to 1.24
```
oc rollout undo deploy/nginx
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/a5926b72-c46e-48f0-9fc4-003aeba9b175)

To cross-check what image version the nginx pods are using after rollback, you can try the below command
```
oc edit pod/nginx-669d5c7ff9-qzt7k
```

Expected outt
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/aa46cbc2-3457-4a40-943e-6edf8575dbe8)

## Lab - Creating a clusterip internal service using declarative style
```
oc expose deploy/nginx --type=ClusterIP --port=8080 --dry-run=client -o yaml
```

Let's redirect the output to a file
```
oc expose deploy/nginx --type=ClusterIP --port=8080 --dry-run=client -o yaml > nginx-clusterip-svc.yml
```

Expected output
Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/f3e20d12-9d76-4004-949e-7367b17b1e20)

Let's create the ClusterIP Internal service using the yaml file
```
cd ~/openshift-sep-2023
git pull
cd Day3/declarative-manifests
oc create -f nginx-clusterip-svc.yml --save-config
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/fd57f149-9d7a-4750-b0e1-e8f5d6d5a0b8)

## Lab - Creating a nodeport external service for nginx deployment in declarative style
```
oc delete -f nginx-clusterip-svc.yml
oc expose deploy/nginx --type=NodePort --port=8080 --dry-run=client -o yaml > nginx-nodeport-svc.yml
cat nginx-nodeport-svc.yml
```
Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/61a4686f-320c-454a-964d-4ecb35149560)

You could create the nodeport sevice as shown below
```
cd ~/openshift-sep-2023
git pull
cd Day3/declarative-manifests
oc apply -f nginx-nodeport-svc.yml
oc get svc
oc describe svc/nginx
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/671324f7-7c68-440f-9f21-b79909d3816a)


## Lab - Creating a LoadBalancer service in declarative style

Let's delete the existing service before creating loadbalancer external service
```
cd ~/openshift-sep-2023
git pull
cd Day3/declarative-manifests
oc delete -f nginx-nodeport-svc.yml
```

Let's create the loadbalancer service
```
cd ~/openshift-sep-2023
git pull
cd Day3/declarative-manifests

oc expose deploy/nginx --type=LoadBalancer --port=8080 --dry-run=client -o yaml > nginx-lb-svc.yml
cat nginx-lb-svc.yml
oc apply -f nginx-lb-svc.yml
oc get svc
oc describe svc/nginx
```
Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/815c91ab-7ee8-45ce-9ba0-ac0b72e9f274)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/6cc1a57c-983b-4630-a4e6-5374e3df14ea)

## Info - What is Service?
- service represents a group of similar pods that belongs to a single deployment in Kubernetes/OpenShift
- they are load-balanced by default
- they are highly available
- application that run in Kubernetes/OpenShift aren't supposed to invoke Pod by IP/host, as Pods can be deleted/replaced any point of time
- so the only reliable/stable abstraction your application can depend is the service
- when a deployment is scaled up/down, the service pod endpoints are automatically updated, hence your application is decoupled from the Pod instance, this make it loosely coupled design

## Info - How to decide which type of service is suitable for a application deployment
There are 3 types of Services in Kubernetes/OpenShift
1. ClusterIP Internal Service
2. NodePort External Service
3. LoadBalancer External Service

### CluterIP Internal Service
- This type of Service is typically use for database deployments
- This service can only be accessed from within the OpenShift cluster
- As the application that needs database access runs within the OpenShift cluster, the ClusterIP db service will work perfectly
- This is inbuilt feature of Kubernetes/OpenShift, hence there won't be any additional cost implications when your create ClusterIP Services either locally or in public cloud based OpenShift cluster

### NodePort External Service
- This type of Service is required to access the application from outside the OpenShift cluster
- Typically for any front-end application or Microservices will require this type of service
- Kubernetes/OpenShift has reserved 30000 to 32767 Port range on every Node in the Cluster for NodePort services
- For each NodePort service we create in the OpenShift cluster, OpenShift will assign a port out of the range 30000-32767 whichever port is available on all the nodes in cluster
- The NodePort will be opened up in every node in the OpenShift cluster
- So each time you create a nodeport service it opens that port on every server in the OpenShift cluster, this might lead to some security issues
- Also the way we access the nodeport service is not end-user friendly, hence this is suitable only for front-end application. Frontend application or Microservices can make use of NodePort.

### LoadBalancer External Service
- This type of Service is used when OpenShift runs in public cloud like AWS/Azure/GCP/Digital Ocean, etc.,
- Each LoadBalancer service will create an External Load Balancer within AWS/Azure, hence there would be dedicated Server where the external Load Balancer works, unlike NodePort service or ClusterIp Service
- Hence, LoadBalancer Service is more reliable
- There is a cost implication each time you create a LoadBalancer Service in public cloud, as the public cloud vendor will charge on monthly basis for the LoadBalancer service
- Loadbalancer also gets allocation one unique static ip, which is accessible over Internet, hence only one person/company can own that unique static ip, so there is a cost from static ip as well


## Lab - Creating a external route as an alternate for NodePort service

### Things to note
- Route is only supported by OpenShift and not available in Kubernetes
- Route provides a public url that is accessible outside the cluster
- this serves as an alternate to nodeport
- this is based on Ingress feature in Kubernetes
- Ingress is not a service, it is a routing rule used in Load Balancer
- the Ingress Controller picks the Ingress rule and configures the Load Balancer with those routing rule
- Ingress supports routing calls to multiple different services, while the OpenShift route generally routes/forwards the call to just one service

Let's delete the existing lb service
```
cd ~/openshift-sep-2023
git pull
cd Day3/declarative-manifests
oc delete -f nginx-lb-svc.yml
```

Let's create a ClusterIP Internal Service
```
cd ~/openshift-sep-2023
git pull
cd Day3/declarative-manifests
oc apply -f nginx-clusterip-svc.yml
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/b8a126f3-6c16-4bb6-a8d6-382093c8a82f)


Let's expose the service via route to make it accessible outside the cluster with a public url
```
cd ~/openshift-sep-2023
git pull
cd Day3/declarative-manifests

oc expose svc/nginx --dry-run=client -o yaml > nginx-route.yml
cat nginx-route.yml
oc create -f nginx-route.yml --save-config
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/de1de0bc-7919-423d-80f3-af3b2cbd1dd5)

Accessing the external route
```
oc get route
curl http://nginx-jegan.apps.ocp.tektutor-ocp-labs:80
```
Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/dc6eacc2-bfdb-4a6c-ae01-5da91f0d4f41)


## Lab - Creating a Pod without replicaset/deployment

### Things to Note
- Creating a Pod without replicaset/deployment is considered a bad practice
- In case the Pod crashes, as there is no replicaset to monitor and repair, we need to manually fix the error
- Hence, this must be done only for testing purpose and not in production

```
oc run hello --image=tektutor/spring-tektutor-helloms:latest
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/39f4ae31-6d9f-49ce-a48d-a470006382d7)

