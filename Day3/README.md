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
