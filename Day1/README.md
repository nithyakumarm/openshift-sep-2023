# Day1

## What is dual/multi booting with boot loader utilities?
- boot loader is a system utility that is installed in Master Boot Record(MBR) in your hard disk
- Master Boot Loader is typically 512 bytes, hence the boot utility size will have fit within the 512 bytes
- Examples
  - LILO
  - GRUB 1
  - GRUB 2
- when a system is booted, the BIOS(Basic Input Output System) - POST(Power On Self Test) once completed, the BIOS will instruct the CPU to run the boot loader utility
- the boot loader utility then scans your hard disk looking for OS, if there are multiple OS installed on your system, then it gives a menu for you to choose the OS you wish to boot into
- though many OS can be installed only one OS can be active at point of time

## What is Hypervisor?
- is virtualization technology
- supports running many Operating System on the same laptop/desktop/workstation/server
- many OS can be active at the same time
- hardware/software technology
- Processors
  - AMD (Advanced Micro Devices) - General Purpose Processors - Virtualization Feature is called AMD-V
  - Intel - General Purpose Processors - Virtualization Feature - VT-X
  - Apple Silicon(ARM Processor) - Embedded
- there are 2 types
  - Type 1
    - Bare-metal hypervisor
    - no OS is required to installed this type of Hypervisor
    - meant for Servers
  - Type 2
    -  For laptops/desktops/workstations
    -  requires Host OS ( could be any of these OS - Unix, Linux, Mac or Windows )
- this type of virtualization is called heavy-weight virtualization
  - because each Virtual machine has to be allocated with dedicated hardware resources
    - CPU Cores
    - RAM
    - Storage - Hard disk
    - Graphics Card
    - Network Card
- each Virtual machine represents one fully functional Operating System
- Examples
  - VMWare
    - Fusion ( Mac OS-X )
    - Workstation ( Linux & Windows )
    - vSphere/vCenter - Bare-metal Hypervisors
  - Oracle VirtualBox - Free works in Linux/Mac/Windows
  - Parallels ( Mac OS-X )
  - Microsoft Hyper-V

## What is Container Technology?
- is an application virtualization technolgy
- each container represents one application process
- though it may look and behave like OS in some cases, technically it is an application
- it doesn't have OS Kernel
- one container represents one application
- many containers can run within an OS
- this type of virtualization is consider lightweight virtualization as they don't require dedicated hardware resources
- containers running in the same OS shares the hardware resources in that OS just like how other normal applications share hardware resources
- containers depend on Linux Kernel Features
  1. Namespace
     - helps in isolating one container from other containers
  2. Control Groups (CGroups)
     - used to apply resource quota restrictions
      - we can limit how cpu cores at the max a container can utilize
      - we can limit how much RAM at the max one container can utilize
 
## What is a Container Engine?
- a high-level user-friendly software that manages container/images
- it depends on Container Runtime to manage containers
- Examples
  - Docker
  - Podman
- Docker Container Engine depends on runC Container Runtime
- Podman Container Engine depends on CRI-O Container Rutnime

## What is Container Runtime?
- a software that manages containers
  - creating a container
  - listing a container
  - stop/start/restarting/kill/abort/delete container
- depends on OS Kernel to create/manage containers
- they are low-level software which isn't user-friendly
- hence normally no end-users directly use the container runtime
- Examples
  - runC 
  - CRI-O
  
## What is Docker?
- a high-level software that depends on Linux Kernel to support application virtualization
- a Container Engine
- it is user-friendly tool to manage docker images/containers
- is developed in Go programming languaged by the company Docker Inc
- comes in 2 flavours
  1. Docker Community Edition - Docker CE ( Free )
  2. Docker Enterprise Edition - Docker EE ( Paid )
- follows client/server architecture
  client tool
  - docker
  server tool
  - runs as a service called dockerd in linux
- the end-users need not have to know low-level kernel stuffs to work in containers
- Docker depends on containerd which depends on Container Runtime called runC

## Hypervisor High-Level Architecture
![Hyper Architecture](HypervisorHighLevelArchitecture.png)

## Docker High-Level Architecture
![Docker Architecture](DockerHighLevelArchitecture.png)

## What is Docker Local Registry?
- is a directory in your local machine
- in Linux machines /var/lib/docker is the directory that acts as a Local Docker Registry
- Docker Registries has a collection of many Docker Images

## What is Docker Private Registry?
- Docker Private Registry has collection of many Proprietary Docker Images and other third party open source and paid images
- this can be setup using either JFrog Artifcatory or Sonatype Nexus
- for testing/learning purpose you could also try registry:2 docker image from Docker Hub Remote Registry
- this is setup for your entire organization, so that images can be shared by all the teams with your company

## What is Docker Remote Registry?
- aka Remote Docker Registry
- it is a Website maintained by Docker Inc organization
- it has many open-source and third-party docker images
- the images can be freely downloaded and used by anyone
- Website url - hub.docker.com

## What is Docker Image?
- it is similar to Windows ISO Operating System image we burn in DVDs
- using the Windows 11 Installer DVD/ISO Image, we can install Windows 11 in many laptops/desktops/workstation/servers
- similar to that, Docker Image is required to create containers
- Example
  - Using mysql docker image, we can create 1-to-many mysql docker containers
  - Using nginx docker image, we can create many nginx docker containers
- in other words, it is a specification of Docker container

## What is Docker Container?
- running instance of a Docker Image
- each container represents one application
- container is not OS
- container has its own file sytem
- container has its own port range ( 0 - 65535 )
- container has its own network stack ( 7 OSI Layers )
- container has its own virtual network card
- container has its own Private IP address
- it has one application binary with all its dependent libraries pre-installed
- it comes with basic minimum tools

## What type of applications can be containerized?
- Web Server
- Application Server
- DB servers
- REST API
- SOAP API
- Message Queue Servers
- Microservices, etc.,

# Docker Commands

## Demo - Installing Docker CE in CentOS 7.9
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/8c9455c9-99cd-4171-a261-efd3f0ae0b02)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/a71e762f-6d8b-4266-a79d-3272fd47de8b)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/d6ff1893-9679-489f-a383-a7b297fbae86)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/4d9406b3-9250-490f-978b-8f630deb70e5)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/5e4fa331-f199-44b7-9cec-f5c0898e07f4)

## Lab - Checking your docker version on the CentOS Lab machine
```
docker --version
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~/openshift-sep-2023]
└─$ docker --version
Docker version 20.10.25+dfsg1, build b82b9f3
</pre>

## Lab - Listing docker images in your local docker registry
```20.10.25+dfsg1, build b82b9f3

docker images
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/f4646949-b5c3-4d53-82b0-713edbbda33c)

## Lab - Docker Remote Registry
Navigate to the below URL in your CentOS web browser
<pre>
http://hub.docker.com 
</pre>

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/e98c7a4f-6706-46dd-90d7-1abb3e41cf59)

Click on Explore link in the hub.docker.com website, you will get below page
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/1db5e123-6a98-4c2b-ac1a-ca682eba923f)

## Lab - Download docker image from Docker Hub Remote Registry to Docker Local registry
```
docker pull mysql:latest
docker images
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/e9d2409a-7fe9-44dd-81a7-c70565c25008)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/941025cd-73fc-4d8d-b2f2-443b9f222a7a)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/3f2b2001-b748-4b41-9371-e781fd77dd1a)

## Lab - Deleting docker image from your local docker registry
```
docker images
docker rmi hello-world:latest
docker images
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/5cca3e7e-3984-47eb-94cd-361d0cc0ab32)


## Lab - Creating your first container
Listing and check if hello-world:latest docker image is present in your local docker registry
```
docker images
```
Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~/openshift-sep-2023]
└─$ docker images                
REPOSITORY                     TAG        IMAGE ID       CREATED         SIZE
tektutor/ansible-centos-node   latest     758c48f5a98f   5 days ago      428MB
tektutor/ansible-ubuntu-node   latest     7e779b4b95bb   6 days ago      220MB
nginx                          latest     eea7b3dcba7e   3 weeks ago     187MB
ubuntu                         22.04      c6b84b685f35   3 weeks ago     77.8MB
mysql                          latest     99afc808f15b   4 weeks ago     577MB
centos                         7.9.2009   eeb6ee3f44bd   24 months ago   204MB
ubuntu                         16.04      b6f507652425   2 years ago     135MB
</pre>


You can now create a container
```
docker run hello-world:latest
```

Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~/openshift-sep-2023]
└─$ docker run hello-world:latest                                                                            
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
719385e32844: Pull complete 
Digest: sha256:dcba6daec718f547568c562956fa47e1b03673dd010fe6ee58ca806767031d1c
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/  
</pre>

You can now check the above command first downloaded hello-world:latest image into your local registry
```
docker images
```
Expected output
<pre>
┌──(jegan㉿tektutor.org)-[~/openshift-sep-2023]
└─$ docker images                
REPOSITORY                     TAG        IMAGE ID       CREATED         SIZE
tektutor/ansible-centos-node   latest     758c48f5a98f   5 days ago      428MB
tektutor/ansible-ubuntu-node   latest     7e779b4b95bb   6 days ago      220MB
nginx                          latest     eea7b3dcba7e   3 weeks ago     187MB
ubuntu                         22.04      c6b84b685f35   3 weeks ago     77.8MB
mysql                          latest     99afc808f15b   4 weeks ago     577MB
<b>hello-world                    latest     9c7a54a9a43c   4 months ago    13.3kB</b>
centos                         7.9.2009   eeb6ee3f44bd   24 months ago   204MB
ubuntu                         16.04      b6f507652425   2 years ago     135MB  
</pre>

You can see the running containers with the below command
```
docker ps
```

If you wish to see all containers even if they aren't running
```
docker ps -a
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/aa640a48-c061-436f-9926-14cdebe68f96)


## Lab - Renaming an existing container 
```
docker rename <old-name> <new-name>
docker rename vigorous_panini hello1_container
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/bf9e99c7-27c6-4321-a7bf-bc57d207a890)

## Lab - Creating a container with specific name and hostname
```
docker run --name jegan-hello-container --hostname jegan-hello-container hello-world:latest 
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/635dc749-124c-42ab-a8b6-57dc300ea49c)

## Lab - Deleting exited containers
```
docker ps -a
docker rm hello1_container jegan-hello-container
docker ps -a
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/fdd78e9e-bf8f-4807-acad-9208c26cce1f)

## Lab - Creating containers and run them in the background
```
docker run -dit --name ubuntu1 --hostname ubuntu1 ubuntu:22.04 /bin/bash
docker run -dit --name ubuntu2 --hostname ubuntu2 ubuntu:22.04 /bin/bash
docker ps
```

In the above command,
<pre>
dit - means deattached interactive terminal i.e run the container in the background
name - name of the container
hostname - any user-defined hostname you wish to assign to your container
ubuntu:22.04 - is the docker image using which the container will be created
/bin/bash - indicates we would to run/launch bash terminal inside the container as default application
host  
</pre>

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/c4276915-91ed-4fdb-9c4f-2a0744e98f52)

## Lab - Getting inside container that is running in background
```
docker ps
docker exec -it ubuntu1 /bin/bash
hostname
hostname -i
ls
exit
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/efd26550-6784-4eaf-9e67-a4f4627fb68e)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/729153cd-92b7-4e55-ae3a-363d02fc2887)
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/d1491f78-db3f-4b1e-9003-31c40df8700e)

## Lab - Creating a container in interactive mode
```
docker run -it --name ubuntu3 --hostname ubuntu3 ubuntu:22.04 /bin/bash
exit
```

Exiting from the shell will lead to exitting the container, as /bin/bash is the default application running inside container.

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/235a1f22-f577-41d4-82ae-ceb53679daa9)

## Lab - Starting an exited container
```
docker ps
docker ps -a
docker start ubuntu3
docker ps
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/4e3a505b-7b66-4661-8fad-5c1e6d9df2f0)

## Lab - Starting multiple containers
```
docker ps
docker ps -a
docker start ubuntu1 ubuntu2 ubuntu3
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/33bd8364-b53f-461c-8c94-530b63bb54d9)


## Lab - Stopping running containers
```
docker ps
docker stop ubuntu1
docker ps
docker stop ubuntu2 ubuntu3
docker ps -a
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/a238356b-538c-48b1-bf60-e5aa6a25a46b)

## Lab - Stopping and Starting multiple containers without calling out their names
```
docker ps
docker ps -q
docker stop $(docker ps -q)
docker ps
docker ps -a
docker ps -aq
docker start $(docker ps -aq)
docker ps
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/922a5ca1-1c4e-416c-a3a2-6d0b9f272e86)

## Lab - Deleting multiple containers gracefully without calling out their names
```
docker ps
docker rm $(docker ps -q)
docker stop $(docker ps -q) && docker rm $(docker ps -aq)
docker ps -a
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/d90a086a-bec0-4d88-a266-f831b0e5a2b4)

## Lab - Deleting multiple containers forcibly without calling out their names
```
docker run -dit --name ubuntu1 --hostname ubuntu1 ubuntu:22.04 /bin/bash
docker run -dit --name ubuntu2 --hostname ubuntu2 ubuntu:22.04 /bin/bash
docker run -dit --name ubuntu3 --hostname ubuntu3 ubuntu:22.04 /bin/bash
docker ps
docker rm -f $(docker ps -aq)
docker ps -a
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/80e21c66-cd65-40fa-ac84-800ad5d7c8d3)

## Lab - Finding IP address of a running container
```
docker ps
docker inspect ubuntu1 | grep IPA
docker inspect -f {{.NetworkSettings.IPAddress}} ubuntu2
docker inspect -f {{.NetworkSettings.IPAddress}} ubuntu3
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/c7d349b8-5b1d-47f6-99f9-aba2724460ed)

## Lab - Creating 3 centos containers
```
docker ps
docker run -dit --name centos1 --hostname centos1 centos:centos7.9.2009 /bin/bash
docker run -dit --name centos2 --hostname centos2 centos:centos7.9.2009 /bin/bash
docker run -dit --name centos3 --hostname centos3 centos:centos7.9.2009 /bin/bash
docker ps
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/b460edae-4526-413a-a39d-7f644ce20580)

## Lab - List containers name that matches a name pattern
```
docker ps -f "name=ubuntu"
docker ps -f "name=centos"
docker ps -f "name=c"
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/d69d5d3b-516d-4de1-a1dc-b94cdcf2202b)

## Lab - Listing all containers whose image name matches a given value
```
docker ps | grep centos:centos7.9.2009
docker ps | grep ubuntu:22.04
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/8ec7334d-1887-4df7-b50f-f955556f4103)

## Lab - Creating a mysql db server container
When prompts for password, type "root@123" without the double quotes.
```
docker ps -a
docker run -d --name mysql --hostname mysql -e MYSQL_ROOT_PASSWORD=root@123 mysql:latest
docker ps
docker exec -it mysql sh
mysql -u root -p
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/f95d867c-3e92-4a46-ae2d-093f1cf76859)

Checking default databases in mysql server and creating a new database named tektutor
```
SHOW DATABASES;
CREATE DATABASE tektutor;
SHOW DATABASES;
USE tektutor
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/9ad16cf5-afda-4d8f-b948-f097e4613446)


Creating table inside tektutor database and inserting some records into training table.
```
CREATE TABLE training ( id INT NOT NULL, name VARCHAR(250), duration VARCHAR(250), PRIMARY KEY(id) );
SHOW TABLES;
INSERT INTO training VALUES ( 1, "DevOps", "5 Days" );
INSERT INTO training VALUES ( 2, "Advanced OpenShift", "5 Days" );
INSERT INTO training VALUES ( 3, "Advanced Ansible", "5 Days" );
SELECT * FROM training;
exit
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/7bda7a7c-2bb4-42ed-b2e7-5af6a1011adc)


## Lab - Checking mysql db server logs
```
docker ps
docker logs mysql
```

Expected output
![image](https://github.com/tektutor/openshift-sep-2023/assets/12674043/f0fb56a2-56bb-443e-b605-6396911f0a99)
