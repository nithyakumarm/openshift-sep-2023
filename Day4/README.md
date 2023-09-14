# Day 4

## Lab - Deploying an application using GitHub Repo source code
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
