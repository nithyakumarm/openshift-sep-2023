# Day 4

## Lab - Deploying an application using GitHub Repo source code using Docker strategy
```
oc project jegan
oc new-app https://github.com/tektutor/spring-ms.git
oc expose service/spring-ms
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/dee9d503-d24b-48e3-92d8-014f754f756d)

To check the build logs in command line, you can try this
```
oc logs -f build/spring-ms-1
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/fc1ffa47-19a8-4bef-b53b-eaa554257f08)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/f53ec5b6-a66d-4078-9e11-53896d2718bf)

You can check the Developer view Topology
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/3527bb63-ebcd-43e4-988d-e297c2fe4c05)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/c6fce17a-e34c-4e2f-a0ae-5f983592e5c0)

## Lab - Starting a build from command-line using buildconfig
```
oc get buildconfigs
oc get buildconfig
oc get bc
```

Start a build from buildconfig
```
oc start-build bc/spring-ms
oc logs -f bc/spring-ms
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/fa3e7fc6-9330-4433-ad15-29f5a7d7fa9b)


Create a route for spring-ms application deployment
```
oc get svc
oc expose svc/spring-ms
```

## Lab - Deploying our custom application into OpenShift using source strategy
```
oc delete project/jegan
oc new-project jegan
oc new-app registry.access.redhat.com/ubi8/openjdk-11~https://github.com/tektutor/spring-ms.git --strategy=source
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/c5390ed3-1584-4310-adbc-663848dcfb54)

Checking the build log
```
oc logs -f bc/spring-ms
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/4cfd0edf-6e43-4e93-98ca-7ca5e7c4f03b)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/fc099ea9-1507-4d14-9afb-29b789b23c85)


Let's create a route for the spring-ms service
```
oc get svc
oc expose svc/spring-ms
oc get routes
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/286cdcb3-250a-427a-a5a7-0ddc7faf7e74)

Now you may access the route from Developer context Topology from your web browser on the CentOS Lab machine.
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/5e707c1b-d506-42e3-9b6d-646e4ae39302)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/5a24dbad-825d-4744-b591-66a6ee98018d)

## Lab - Deploying custom appling using Docker Hub custom image
```
oc delete project/jegan
oc new-project jegan
oc new-app tektutor/spring-ms:1.0
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/d78cc75f-a05d-4400-94e7-ccc28fe29ff4)

Let us check the deployment status
```
oc status
oc get svc
oc expose svc/spring-ms
oc get route
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/fea6ce87-ae87-47d1-a17a-5934639dca19)

Now you may access the application using its route url from the OpenShift webconsole
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/bb7c5c0f-cc8b-46b8-a943-7254560d7bcc)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/29afcfd7-106b-403e-b09c-321ca7ba0731)

## Lab - Node Affinity Required criteria

When creating the pod, we have added some criteria for the default-scheduler to follow.  When the criteria is required, the scheduler will look for nodes that has a label matching "ssd" type disk, in case it isn't able to find a node that has label disk=ssd then the Pod will not be deployed.

```
cd ~/openshift-sep-2023
git pull
cd Day4/node-affinity
oc apply -f pod-with-node-affinity-required.yml
oc get po
oc describe pod/hello
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/4bdd17f3-a156-423c-8c3e-938a697da2c8)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/6cf0e94e-10a3-45c9-b8de-968238339fba)

Now let's add the label to worker-1 node and see the Pod getting deployed into worker-1.
```
oc label node/worker-1.ocp.tektutor-ocp-labs disk=ssd
oc get po -w
oc get po -o wide
oc get nodes -l disk=ssd
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/e172d74b-77f5-4ee5-bc33-dddd5912a7ea)

## Lab - Pod manifest has a preferred criteria i.e node has label disk=ssd

How the node affinity works in case of Preferred ?
- The scheduler will search for nodes that has label disk=ssd, if it is able to find a node that has the label then the Pod will be scheduler there
- If the scheduler isn't able to find such a node, the scheduler will deploy it on any node as per scheduler's choice

```
cd ~/openshift-sep-2023
git pull
oc delete pod/hello
cd Day4/node-affinity
oc apply -f pod-with-node-affinity-preferred.yml
oc get nodes -l disk=ssd
oc get po
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/2db7344b-7c2b-4434-b986-ac3764a7b031)

## Info - Docker Network Model

## Info - OpenShift/Kubernetes Network Model

## Post Test Link
https://app.mymapit.in/code4/tiny/yTefEw

## Feedback link :
https://tcheck.co/HgqK27


## Installing OpenShift on your laptop
https://developers.redhat.com/products/openshift-local/overview

