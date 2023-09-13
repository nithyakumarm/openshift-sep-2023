# Day 3

## Lab - Understanding Service Discovery in OpenShift/Kubernetes
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
