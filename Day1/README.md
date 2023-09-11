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
- the end-users need not have to know low-level kernel stuffs to work in containers
- Docker depends on containerd which depends on Container Runtime called runC

## Hypervisor High-Level Architecture

## Docker High-Level Architecture

## What is Docker Local Registry?

## What is Docker Private Registry?

## What is Docker Remote Registry?

## What is Docker Image?

## What is Docker Container?


