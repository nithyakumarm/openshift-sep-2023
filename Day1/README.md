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
