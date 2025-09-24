# Kubernetes 

Kubernetes is a open source container orchestration tools, developed by **google**.
- It helps us manage containerized applications like Docker containers.
    - For example, if you have multiple containers that need to be managed, Kubernetes is the tool for that.
    - It can manage containers in multiple environments, such as on-premises, virtual machines (VMs), or the cloud or even hybrid environment.

## Why use Kubernetes: what does it solve.

The rise of use of **micro services** caused the rise of usage of containers. and managing 100s or containers using scripts or self made tools made it more complex and borderline impossible to manage those containers.

This is were container orchestration tools like kubernetes comes in.

## what features do orchestration tools offers?

- High availability.
- Scalability or high performance.
- Disaster recovery.

# Kubernetes Components

## Pod
A Pod in Kubernetes is the smallest deployable unit and represents a group of one or more containers (such as Docker containers) that share storage, networking, and configuration settings. All containers in a Pod are tightly coupled: they run on the same Node, share the same IP address and storage volumes, and can communicate with each other using localhost. Kubernetes creates and deletes Pods as needed, for example, to scale up or down or recover from failures.

- pod is a abstraction over the container( it is a layer over the container)
- Usually 1 application per pod
- New IP address on the re-creation


## Node

A Node is a physical or virtual machine that serves as a worker in a Kubernetes cluster. Each Node runs one or more Pods and provides the resources (CPU, memory, storage, networking) needed to run those Pods.

## Service:

- Service is a static ip address.
- as there is a lifecycle for the pod i.e., if the pod gets re-created a new IP address is assigned to the pod. this is inconvenient as this means that we need to again map the db with the new ip address, this is where the **service** comes in as this is a static ip address, and the service has no lifecycle, means that even if the pod dies the service does not. 


## Ingress

 
# Kubernetes Architecture:

## Cluster:



