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


 
# Kubernetes Architecture:

## Cluster:

A **Kubernetes cluster** is a group of machines (nodes) managed together, where Kubernetes orchestrates and runs containerized applications


### Key Components


**Control Plane:** The “brain” that manages the whole cluster, scheduling workloads, scaling, maintaining desired state, and exposing the Kubernetes API.

**Worker Nodes:** These are servers (physical or virtual) that run your application Pods (containers).

**Pods:** The smallest unit, which are groups of one or more containers running on nodes.

#### How Clusters Work?
Kubernetes clusters enable you to run applications at scale—if one node fails, your app can keep running on other nodes. The control plane makes decisions and tells worker nodes what to do, while worker nodes do the actual work by running containers inside Pods.

## Control Plane

The control plane is the "brain" of Kubernetes and is responsible for managing cluster state, scheduling deployments, scaling workloads, and enforcing configuration. Key components include:

**API Server (kube-apiserver):** The cluster’s entry point, serving the Kubernetes API for handling requests.

**etcd:** Distributed key-value store holding cluster state and configuration.

**Scheduler (kube-scheduler):** Assigns pods to suitable nodes based on resource needs and constraints.

**Controller Manager:** Runs built-in controllers to keep the cluster in its desired state, e.g., managing Replicates and Nodes.

**Cloud Controller Manager:**
Integrates with cloud provider APIs to provision resources like load balancers.

The control plane communicates with worker nodes to tell them what to do (create/delete pods, monitor health, etc.)


## Data Plane

The data plane consists of the worker nodes and their components, which actually run the application containers. Each node manages resources, runs pods, and maintains networking:

**Kubelet:** Ensures containers/pods are running according to the control plane’s instructions.

**Kube-proxy:** Handles networking rules for service communication.

**Container Runtime:** Runs the actual containers (Docker, containerd, etc.).

The data plane executes work as dictated by the control plane.


# Kubernetes Interfaces:

- Container Runtime Interface(CRI)
- Container Network interface(CNI)
- Container Storage interface (CSI)



# Kubernetes Resources:

## Namespace:
- Provide a mechanism to group resources within a cluster.
- There are 4 initial namespaces: 
    - default
    - kube-system
    - Kube-node-lease
    - kube-public

- by default, namespaces **DO NOT act as a network/security boundary.**







