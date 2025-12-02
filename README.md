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


## Data Plane or worker nodes

The data plane consists of the worker nodes and their components, which actually run the application containers. Each node manages resources, runs pods, and maintains networking:

**Kubelet:** Ensures containers/pods are running according to the control plane’s instructions.

**Kube-proxy:** Handles networking rules for service communication.

**Container Runtime:** Runs the actual containers (Docker, containerd, etc.).

The data plane executes work as dictated by the control plane.


# Kubernetes Interfaces:

- Container Runtime Interface(CRI)
- Container Network interface(CNI)
- Container Storage interface (CSI)

---


# Pods:

Pods can be created in Kubernetes using both imperative and declarative way just like in docker,

**Imperative is by running the command:**

```bash
kubectl run nginx
```

**Declarative way is using JSON or YAML file and declaring the state of the pod in it**

## Imperative way of creating a pod:

The cmd to creating a pod is shown below:

```bash
#kubectl run pod-name --image=image-name/version

kubctl run nginx-pod --image=nginx/latest

```
![imperative](./images/image.png)


- Here the pod is first getting created and then in runnig state.
- the **Ready** `1/1` is the number of container in the pod. here it is showing there is one container in the pod.

## Declarative way of creating the pods:

We can use both `json` and `yaml` to write a **Kubernetes** configuration.

here we will be using the `json` format;

``` json
apiVersion: v1
kind : Pod
metadata:
  name: nginx-pod02
  labels:
    env: dev
    cloud: azure
    type: frontend
spec:
  containers:
   - name: nginx-cont-01
     image: nginx:latest
     ports:
      - containerPort: 80
```

- command to create the pod using a config file:

```bash
Kubectl create -f aaaa.yaml
```

- if you have made some mistakes which creating the pod or there is some mistakes in the config file, use the command below:
- - it will show you your pod config details and the information or step taken during the creation of the pod.

```bash
kubectl describe pod pod_name
```

- to live edit the configuration of the pod use the command below:

```bash
kubectl edit pod pod_name
```

- to interact with the pod we can us the below command:

```bash
kubectl exec -it pod_name -- /bin/bash     # for bash
kubectl exec -it pod_name -- sh            # for normal shell
```

 

## Trick to create your on `yaml` or `json` using imperative commands:

it is not always required to write your own `yaml` or `json` file. we can use the imperative command and dry run the creating of pod and ask the kubectl to give the output in `yaml` or `json`. With this output details we can create a file.

### Commands:

```bash
 kubectl run nginx --image=nginx:latest --dry-run=client -o yaml > test_pod_yaml.yaml
 kubectl run nginx --image=nginx:latest --dry-run=client -o json > test_pod_json.json
```

### Outputs:

```bash
root@ip-172-31-27-211:/home/ubuntu/kubernetes# kubectl run nginx --image=nginx:latest --dry-run=client -o yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    run: nginx
  name: nginx
spec:
  containers:
  - image: nginx:latest
    name: nginx
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Always
status: {}
root@ip-172-31-27-211:/home/ubuntu/kubernetes# kubectl run nginx --image=nginx:latest --dry-run=client -o json
{
    "kind": "Pod",
    "apiVersion": "v1",
    "metadata": {
        "name": "nginx",
        "labels": {
            "run": "nginx"
        }
    },
    "spec": {
        "containers": [
            {
                "name": "nginx",
                "image": "nginx:latest",
                "resources": {}
            }
        ],
        "restartPolicy": "Always",
        "dnsPolicy": "ClusterFirst"
    },
    "status": {}
}
root@ip-172-31-27-211:/home/ubuntu/kubernetes# kubectl run nginx --image=nginx:latest --dry-run=client -o yaml > test_pod_yaml.yaml
root@ip-172-31-27-211:/home/ubuntu/kubernetes# kubectl run nginx --image=nginx:latest --dry-run=client -o json > test_pod_json.json

```


- to get the node information for pod use the below commands:

```bash
kubectl get pod -o wide
```

`-o wide` will give the detailed information of the pod which also includes the node details:

```bash
root@ip-172-31-27-211:/home/ubuntu/kubernetes/Kubernetes_Learning# kubectl get pods -o wide
NAME         READY   STATUS    RESTARTS       AGE   IP           NODE               NOMINATED NODE   READINESS GATES
nginx-pod1   1/1     Running   2 (103m ago)   8h    10.244.2.2   cluster01-worker   <none>           <none>
root@ip-172-31-27-211:/home/ubuntu/kubernetes/Kubernetes_Learning# 

```

- we can also use the `describe` command to get the pod details:

```bash
kubectl describe pod pod_name
```

